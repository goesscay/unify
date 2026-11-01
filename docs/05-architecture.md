# 05 — Solution Architecture

Builds on Part C of the blueprint. The core choices stand (modular monolith, FastAPI, PostgreSQL, Temporal, S3, OpenTelemetry). The changes, with reasons, are listed in the ADRs at the end.

## 1. Architecture principles

1. **Modular monolith first.** One deployable API, plus separate worker processes. Module boundaries are enforced in code (packages, no cross-module DB access) so modules can be extracted later.
2. **Workflows own state; agents own reasoning; adapters own side effects.** No component writes to an ERP except an adapter invoked by a workflow activity after a policy check.
3. **Deterministic core, AI at the edges.** Validation, transformation, loading and reconciliation are deterministic code. AI proposes mappings, values, tests and classifications.
4. **Tenant isolation everywhere**: in the DB (RLS), object storage (prefix plus KMS key per tenant at scale), vector index, caches, logs and LLM prompts.
5. **Everything versioned**: playbooks, mappings, rules, adapters, prompts and agent definitions.

## 2. Logical architecture

```
┌───────────────────────────── EXPERIENCE ──────────────────────────────┐
│ Product-company app · Customer portal (white-label) · Admin console   │
│ Public REST API + webhooks · Excel add-in (later)                     │
└───────────────────────────────┬───────────────────────────────────────┘
                                │ HTTPS (OIDC tokens)
┌───────────────────────────────▼───────────────────────────────────────┐
│ API / APPLICATION (FastAPI modular monolith)                          │
│ identity&access · tenancy · projects&playbooks · approvals&policy     │
│ migration · configuration · testing · readiness · knowledge · billing │
│ audit · notifications · files                                         │
└──────┬───────────────┬───────────────────┬────────────────────┬───────┘
       │               │                   │                    │
┌──────▼─────┐ ┌───────▼────────┐ ┌────────▼──────────┐ ┌───────▼───────┐
│ PostgreSQL │ │ Temporal       │ │ Worker pools       │ │ Object store  │
│ +RLS       │ │ (durable       │ │ • data workers     │ │ (S3) uploads, │
│ +pgvector  │ │  workflows,    │ │   (Polars/DuckDB)  │ │ reports,      │
│            │ │  timers,       │ │ • agent workers    │ │ evidence      │
│            │ │  approvals)    │ │ • connector workers│ │               │
└────────────┘ └────────────────┘ └────────┬──────────┘ └───────────────┘
                                           │
                ┌──────────────────────────┼───────────────────────────┐
                │ AI GATEWAY               │ CONNECTOR RUNTIME          │
                │ model routing, PII       │ adapters: Mock · Odoo ref ·│
                │ redaction, prompt        │ one per tenant product …   │
                │ registry, cost metering, │ capability registry,       │
                │ eval hooks, caching      │ rate limiting, idempotency │
                └──────────────────────────┴──────────────┬────────────┘
                                                          │ OAuth / API keys (vault)
                                              ┌───────────▼───────────┐
                                              │ Customer ERP sandbox  │
                                              │ (production only with │
                                              │ explicit authorisation)│
                                              └───────────────────────┘
```

## 3. Technology stack

| Layer | Choice | Notes |
|---|---|---|
| Frontend | Next.js (React, TypeScript), Tailwind + shadcn/ui, TanStack Table/Query, AG Grid (data grids for mapping/exceptions) | Large data grids are central to the UX |
| API | Python 3.12+, FastAPI, Pydantic v2, SQLAlchemy 2 + Alembic | Python for the data and AI ecosystem |
| Data processing | **Polars + DuckDB** in workers; openpyxl/calamine for Excel | Fast profiling/transforms on millions of rows without a Spark cluster |
| Workflows | **Temporal** (Temporal Cloud early to avoid ops; self-host later) | Durable migrations, approvals as signals, retries, timers |
| Agents | Thin in-house agent runtime (tool-calling loop inside Temporal activities) | LangGraph optional later; see ADR-003 |
| AI gateway | In-house abstraction (or LiteLLM) over multiple providers | Model-agnostic; route by task; per-tenant budgets |
| DB | PostgreSQL 16 (AWS RDS/Aurora), RLS, pgvector | One DB cluster per region |
| Cache/queue | Redis (sessions, rate limits) | Temporal handles the job queues |
| Files | S3 + KMS; ClamAV scan on upload | Presigned uploads |
| Identity | Keycloak (self-hosted) **or** a managed B2B auth (WorkOS/Auth0) | ADR-005; enterprise SSO/SCIM needed by GA |
| Secrets | AWS Secrets Manager / HashiCorp Vault | ERP credentials never in the DB |
| Observability | OpenTelemetry → Grafana stack (Tempo/Loki/Prometheus) or Datadog; Sentry | Plus LLM tracing (Langfuse) |
| Infra | AWS: ap-south-1 (Mumbai) first, eu-west-2 (London) next; Kubernetes (EKS) or ECS Fargate; Terraform | ECS is simpler for a small team |
| CI/CD | GitHub Actions; trunk-based; preview environments | Repo: github.com/goesscay/unify |
| Testing | pytest, Playwright, contract tests per adapter, an AI eval harness | See [08](08-ai-agents-governance.md) |

## 4. Module map (code boundaries)

```
unify/
  apps/web/                 # Next.js product-company app + customer portal
  services/api/             # FastAPI modular monolith
    modules/
      identity/ tenancy/ projects/ playbooks/ approvals/ audit/
      files/ migration/ configuration/ testing/ readiness/
      knowledge/ billing/ notifications/
  services/workers/         # Temporal workers: data, agent, connector
  packages/connectors/      # adapter SDK, conformance suite + adapters (mock, odoo, <tenant products>)
  packages/agents/          # agent definitions, tools, prompts (versioned)
  packages/rules/           # deterministic validation/transform library
  packs/                    # ERP knowledge packs + sample playbooks (YAML)
  infra/                    # Terraform, Helm/ECS definitions
  docs/                     # this documentation
```

## 5. Key runtime flows

### 5.1 Governed action lifecycle (every write)
`Propose (agent/user) → Policy check (allowed op? env? materiality?) → Approval (Temporal signal from a human) → Execute (connector activity, idempotency key) → Verify (read-back/diff) → Audit (append-only event + evidence in S3)`

### 5.2 Migration workflow (Temporal)
`MigrationWorkflow(job)` → `ProfileActivity` → `SuggestMappingActivity` (agent) → *await MappingApproved signal* → `TransformValidateActivity` → *await exceptions resolved or waived* → *await LoadApproved* → `LoadBatchActivity × N` (parallel within ERP rate limits; heartbeat; retry with the same idempotency key) → `ReconcileActivity` → *await ReconciliationSignOff* → `EvidencePackActivity`.

### 5.3 Multi-tenancy model
- **Pooled** multi-tenant by default: shared DB with `tenant_id` + RLS (`SET app.tenant_id` per transaction from the token).
- **Siloed** option for Enterprise: a dedicated DB/schema and dedicated workers in the customer's chosen region, or a VPC deployment.
- Tenant = ERP product company. Its customers' data stays inside that tenant, scoped by `customer_id`/`project_id`.
- Adapters for a tenant's product run only for that tenant; adapter code and credentials are never shared across tenants.

## 6. Environments

| Env | Purpose | Data |
|---|---|---|
| local | Developer machines (docker compose: Postgres, Temporal, MinIO, Odoo, mock ERP) | Synthetic |
| dev / preview | Per-PR previews | Synthetic |
| staging | Pre-release, pen tests, design-partner UAT | Anonymised |
| prod-in | India region | Real |
| prod-eu | UK/EU region (by GA) | Real |

## 7. Architecture Decision Records (initial)

| ADR | Decision | Rationale |
|---|---|---|
| ADR-001 | Modular monolith + workers | Small team; keep boundaries for later extraction (blueprint) |
| ADR-002 | Temporal for durable workflows | Long-running, approval-gated, retry-heavy processes are Temporal's core use case. Use Temporal Cloud first. |
| ADR-003 | **Defer LangGraph**; agents are bounded tool-calling loops inside Temporal activities | Two orchestration engines add complexity; Temporal already provides state, human-in-the-loop and retries. Revisit if agent graphs become complex. |
| ADR-004 | **Model-agnostic AI gateway** (not OpenAI-only) | Route by task, cost and region; avoid lock-in; run evals across providers; allow enterprise BYO keys |
| ADR-005 | Identity: decide between Keycloak and managed B2B auth in Sprint 0 | Managed = faster SSO/SCIM; Keycloak = lower cost and self-hosting for private deployments |
| ADR-006 | Polars/DuckDB for data plane; no Spark | Mid-market volumes (≤ tens of millions of rows) fit on single nodes |
| ADR-007 | AWS Mumbai first | India-based team and early customers; DPDP; latency |
| ADR-008 | Adapters run in isolated worker pools with per-adapter egress allow-lists | Limits blast radius of a compromised or buggy adapter |
| ADR-009 | Excel round-trip is a first-class feature | Adoption: consultants live in Excel |
