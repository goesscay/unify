# 07 — Product Connector Framework

> "The connector is the key to product independence." (SME blueprint, section 10)

Every ERP product company's product connects to UNIFY through an **adapter** that implements one standard contract and exposes only documented, allow-listed capabilities. AI must never invent API endpoints, database fields or configuration options.

## 1. Adapter contract (v1)

Extends the blueprint's standard operations (`authenticate`, `get_product_metadata`, `get_supported_modules`, `get_entity_schema`, `validate_records`, `create_records`, `update_records`, `get_records`, `execute_configuration`, `execute_test`, `check_connection`) with dependency planning, idempotency and rate limits.

```python
class ProductAdapter(Protocol):
    # identity & health
    def describe(self) -> AdapterInfo                        # product, version range, declared capabilities
    def authenticate(self, conn) -> Session
    def check_connection(self, conn) -> HealthResult
    # metadata
    def get_product_metadata(self, conn) -> ProductMeta      # version, modules, instance info
    def get_supported_modules(self, conn) -> list[Module]
    def list_entities(self, conn) -> list[EntityMeta]         # incl. load order + dependencies
    def get_entity_schema(self, conn, entity) -> EntitySchema # fields, types, required, refs, enums
    def get_setup_state(self, conn, area) -> dict             # current configuration snapshot
    # data
    def get_records(self, conn, entity, filters, page) -> Page
    def validate_records(self, conn, entity, records) -> list[RecordResult]   # product-side dry run where supported
    def create_records(self, conn, entity, records, *, idem_key) -> list[RecordResult]
    def update_records(self, conn, entity, records, *, key, idem_key) -> list[RecordResult]
    def delete_records(self, conn, entity, ids, *, idem_key) -> list[RecordResult]  # sandbox only, policy-gated
    # configuration
    def plan_configuration(self, conn, items) -> ConfigPlan  # ordered, dependency-checked, diff vs current
    def execute_configuration(self, conn, plan, *, idem_key) -> ConfigResult
    # testing
    def execute_test(self, conn, test_step) -> StepResult
    # limits
    def rate_limits(self) -> RateLimitPolicy
```

**Rules**
- Declared capabilities are stored in the **capability registry** and approved by the product company's Product/API Owner. The agent toolset is generated from the registry. If a capability is not registered and approved, there is no tool for it.
- Calls run in isolated connector workers with credentials fetched from the vault at run time, using least-privilege API scopes.
- Writes are idempotent: the adapter maps `idem_key` to external IDs or performs lookup-before-create.
- Every adapter passes the shared **conformance suite** (mock + real sandbox) before certification, and nightly afterwards.

## 2. Product onboarding: how a new product connects

| Step | Who | Output | Typical effort |
|---|---|---|---|
| 1. API discovery workshop | UNIFY integration engineer + product company's tech lead | API inventory: auth, entities, setup endpoints, bulk import, limits, sandbox provisioning | 1 week |
| 2. Capability spike | UNIFY | Read metadata; create/update/delete core entities; apply 1 setup area; measure throughput | 1–2 weeks |
| 3. Adapter build | UNIFY (later: product company via SDK) | Contract implementation + mapping of product errors to standard errors | 2–4 weeks |
| 4. Knowledge pack | Product company's implementation lead + UNIFY functional consultant | Entities' business meaning, load order, validations, localisations, config templates, test packs, existing migration templates imported | 2–3 weeks (parallel) |
| 5. Conformance + load test | UNIFY | Certified adapter; 100k-record load test | 1 week |
| 6. Security review | UNIFY + product company's CTO | Approved scopes; capability registry published | ≤ 1 week |

**Target: ≤ 8 weeks for the first design partner, ≤ 4 weeks per product by GA, ≤ 2 weeks self-serve (SDK + low-code adapter builder) later.**

### What we ask of the product company (API readiness checklist)
- Documented REST/OData APIs for the core master entities (create, read, update; delete in sandbox)
- APIs or bulk import for key setup areas (masters for tax, payment terms, number series, posting/account groups, units, warehouses)
- Sandbox/test instance per customer (or a cheap way to create one)
- Service-account / OAuth client credentials with scopable permissions
- Published rate limits; webhooks optional
- If APIs are missing for an area: agree a roadmap, or use the product's bulk import format (CSV/Excel template) as the execution channel

## 3. Knowledge pack structure (per product)

```
packs/<product>/
  pack.yaml                 # version, compatible adapter/product versions
  entities/*.yaml           # business meaning, mandatory fields, gotchas, load order
  setup-dependencies.yaml   # e.g. tax setup before customers
  rules/*.yaml              # standard validations (formats, references)
  localisations/in-gst.yaml, ae-vat.yaml, uk-vat.yaml
  config-templates/*.yaml   # baseline setups per industry playbook
  test-packs/o2c.yaml, p2p.yaml, r2r.yaml
  migration-templates/      # the product company's existing Excel templates
  docs/                     # product documentation chunks for retrieval
```

UNIFY also maintains **global packs** that every product benefits from: source-system parsers and mappings (Tally, QuickBooks, Excel conventions), common formats (GSTIN, VAT, IBAN), and generic ERP concepts.

## 4. Adapters for development and demos

The design partner's product is not available on day 1, so:
1. **Mock ERP** (weeks 1–4): a configurable fake product with entities, setup and errors, used for development and automated tests.
2. **Reference ERP adapter: Odoo** (open source, self-hostable in docker): a realistic ERP for end-to-end demos to prospects and for CI, and it proves the contract on a real product.
3. **Design partner's product**: from Phase 0 (spike) and Phase 2 (production-grade adapter).

## 5. Source-side parsers (where customers migrate from)

| Source | Approach | Priority |
|---|---|---|
| Excel/CSV (incl. the product company's own templates) | Native | P0 |
| Tally (India) | Parse XML/Excel exports: masters, ledgers, opening balances | P1 (high value in India) |
| QuickBooks Online / Xero | OAuth APIs | P1 |
| Busy, Marg, Zoho Books, older ERPs | Export templates | P2 |
| Generic SQL source | Read-only extraction agent run by the customer | P2 |

## 6. Connectivity methods (blueprint priorities, kept)

| Method | Use case | Priority |
|---|---|---|
| REST APIs | Product operations | P0 |
| Webhooks | Status and event updates | P0 |
| Product bulk import formats | Large loads/setup where APIs are thin | P0 |
| SFTP | Legacy file exchange | P1 |
| Database connector | Approved legacy data extraction | P1 |
| UI automation | Products without usable APIs; controlled use | P2 |
| MCP | Standardised agent tool access; wrap product MCP servers where they exist | P2 |
