# 07 — ERP Connector Framework

The connector is what makes UNIFY "one engine, any product." This document defines the adapter contract, the capability registry and the ERP rollout order.

## 1. Adapter contract (v1)

Every adapter implements this interface (Python, `packages/connectors/sdk`). All write methods take an idempotency key and return per-record results.

```python
class ErpAdapter(Protocol):
    # identity & health
    def describe(self) -> AdapterInfo                      # erp, version range, capabilities
    def check_connection(self, conn) -> HealthResult
    # metadata
    def list_companies(self, conn) -> list[Company]
    def list_entities(self, conn) -> list[EntityMeta]       # incl. load order, dependencies
    def get_entity_schema(self, conn, entity) -> EntitySchema  # fields, types, required, refs, enums
    def get_setup_state(self, conn, area) -> dict           # current configuration snapshot
    # data
    def read_records(self, conn, entity, filter, page) -> Page
    def validate_records(self, conn, entity, records) -> list[RecordResult]   # adapter-side/ERP dry-run where possible
    def upsert_records(self, conn, entity, records, *, key, idem_key) -> list[RecordResult]
    def delete_records(self, conn, entity, ids, *, idem_key) -> list[RecordResult]  # sandbox only, policy-gated
    # configuration
    def plan_configuration(self, conn, items) -> ConfigPlan  # ordered, with dependencies + diff vs current
    def apply_configuration(self, conn, plan, *, idem_key) -> ConfigResult
    # testing
    def execute_test_step(self, conn, step) -> StepResult   # e.g. post document, read ledger
    # limits
    def rate_limits(self) -> RateLimitPolicy
```

**Rules**
- Capabilities are declared in `describe()` and stored in the **capability registry** (`adapters.capabilities`). The agent toolset is generated from the registry. If a capability is not registered, there is no tool for it.
- Every call is made by a connector worker under a scoped credential fetched from the vault at run time.
- Retries are safe: the adapter maps `idem_key` to ERP external IDs or a lookup-before-create check.
- Contract tests: each adapter must pass the shared conformance suite against a mock and a real sandbox in CI (nightly).

## 2. ERP knowledge pack (per ERP)

```
packs/business-central/
  pack.yaml                 # version, compatible adapter versions
  entities/*.yaml           # business meaning, mandatory fields, gotchas, load order
  setup-dependencies.yaml   # e.g. posting groups before customers
  rules/*.yaml              # standard validations (formats, references)
  localisations/in-gst.yaml, uk-vat.yaml, ae-vat.yaml
  config-templates/*.yaml   # baseline setups by industry
  test-packs/o2c.yaml, p2p.yaml, r2r.yaml
  docs/                     # curated docs chunks for retrieval
```

UNIFY builds and maintains the base pack (global scope). Partners layer **their own packs** on top (tenant scope).

## 3. First ERP: recommendation

| Criterion | **Dynamics 365 Business Central** | Odoo | NetSuite | SAP Business One | SAP S/4HANA |
|---|---|---|---|---|---|
| API quality for write ops | Strong: REST API v2.0, custom APIs via AL extensions, automation APIs incl. configuration packages (RapidStart) | Good: external JSON-RPC/XML-RPC API, any model | Good: SuiteTalk REST, SuiteQL; some setup only via UI or SDF | Good: Service Layer (OData) | Complex; SAP's own migration tooling dominates |
| Sandbox access cost | Low: sandboxes included with tenants and partner programmes | **Free** (open-source Community, self-host) | Paid sandbox; harder to get | Partner licence | Expensive |
| Partner channel size | Very large, global, Microsoft does not implement directly | Large and growing (strong in India, MEA, EU) | Medium; Oracle also implements directly | Large SMB channel (strong in India) | Large SIs, SAP-native AI competing |
| Mid-market fit | Excellent | Excellent | Excellent | Good (SMB) | Enterprise |
| Competitive threat from native AI tooling | Medium (Copilot) | Low | Medium | Low | **High** |

**Recommendation**
1. **Mock ERP adapter** (week 1–4): drives development and tests, with deterministic behaviour.
2. **Odoo reference adapter** (months 1–3): free, self-hostable in docker for CI, a full ERP for end-to-end demos, plus a large partner ecosystem in India.
3. **Business Central production adapter** (months 2–5): first commercial target, with design partners from the BC channel.
4. Next (after pilot evidence): **NetSuite** or **SAP Business One**, based on design-partner demand; then Acumatica and Sage Intacct.

> Decision needed from the founder: which ERP ecosystem does your network know best? If your contacts are SAP B1 or Odoo partners, start there. The best first ERP is the one where you can sign two design partners quickly. See [14](14-risks-assumptions-questions.md).

## 4. Source-side connectors (legacy systems)

Customers migrate *from* somewhere. The source parsers for the most common mid-market sources are a strong differentiator:

| Source | Approach | Priority |
|---|---|---|
| Excel/CSV templates | Native | P0 |
| Tally (India) | Parse Tally XML/Excel exports; masters + ledgers + opening balances | P1 (high value in India) |
| QuickBooks Online / Xero | OAuth APIs | P1 |
| Dynamics NAV/GP, older SAP B1, Sage 50 | DB/flat-file extracts with templates | P2 |
| Generic SQL source | Read-only extraction agent run by the customer, outbound-only | P2 |

## 5. Connectivity methods (from blueprint, re-prioritised)

| Method | Priority | Note |
|---|---|---|
| ERP REST/OData APIs | P0 | Primary |
| ERP bulk/config import (e.g., BC configuration packages, NetSuite CSV import) | P0 | Fastest for large loads and setup |
| Webhooks | P1 | Status events |
| SFTP | P1 | Legacy file exchange |
| On-prem agent (outbound tunnel) | P2 | For on-prem ERPs/sources; needed for enterprise SIs |
| UI automation | P2 | Last resort, strongly governed |
| MCP server (UNIFY exposes tools to external agents; consumes vendor MCP servers) | P2 | Strategic: ERP vendors are publishing MCP servers. Adapters can wrap them later. |

## 6. Adapter development process

1. Capability spike (1–2 weeks): auth, read metadata, create/update/delete in sandbox, rate limits, error model.
2. Implement the contract plus a pack for 6 core entities.
3. Conformance suite green, then a load test (100k records).
4. Security review (scopes, least privilege, secrets).
5. Design-partner beta, then GA.

Target effort per new mid-market ERP adapter (core entities): **6–10 engineer-weeks** once the SDK is mature.
