# 04 — Product Requirements Document (PRD)

Version 0.1 · builds on Part A of the blueprint, re-scoped to the partner-first wedge ([02-product-strategy.md](02-product-strategy.md)).

## 1. Problem statement

ERP implementation teams repeat the same discovery, data migration, configuration, testing and readiness work on every project, using spreadsheets, email and individual expertise. This creates overruns, inconsistent quality, poor traceability and low margins. AI tools today either track the work (PSA/onboarding tools) or help one consultant ad hoc (chatbots). Neither *executes* governed work against the ERP or retains the know-how.

## 2. Goals and non-goals

**Goals (MVP, first 6–7 months after validation)**
- G1. Cut consultant effort on master-data migration and standard configuration by **≥ 30%** on pilot projects, measured against a baseline.
- G2. **100%** of migrated records traceable to their source row or to an explicit exception.
- G3. Zero unauthorised writes; every write is approved, executed through the connector, verified and audited.
- G4. Customer turnaround on data/exception requests reduced by **≥ 40%** via the customer portal.
- G5. Two design partners complete at least 2–3 real projects through UNIFY and convert to paid.

**Non-goals (MVP):** production cutover automation; more than one production-grade ERP adapter; UI/RPA automation; full PSA (timesheets, billing); ongoing integrations (iPaaS); autonomous approvals.

## 3. Personas

| Persona | Org | Goals | Key screens |
|---|---|---|---|
| **Partner Admin** | Implementation partner | Set up workspace, ERPs, users, packs, billing | Settings, Adapters, Packs, Users, Billing |
| **Delivery Head / PMO** | Partner | Portfolio health, margin, go-live predictability | Control Tower (internal dashboard) |
| **Project Manager** | Partner | Run project, approvals, blockers, customer actions | Project workspace, Approvals, Readiness |
| **Functional Consultant** | Partner | Requirements, configuration, fit-gap | Configuration, Requirements, Tests |
| **Data Migration Specialist** | Partner | Mapping, rules, loads, reconciliation | Migration workbench |
| **QA Lead** | Partner | Tests, defects, evidence | Testing |
| **Customer Sponsor / Admin** | End customer | Visibility, approve, upload data | Customer Delivery Workspace (customer portal) |
| **Customer Data Owner / Key User** | End customer | Fix data exceptions, UAT | Customer action centre, exception queue |
| **Vendor Admin** (segment B) | ERP vendor | Publish product adapter and playbooks to partners | Vendor workspace |
| **UNIFY Super Admin** | UNIFY | Tenants, plans, global packs, support | Admin console |

## 4. Core user journey (MVP)

1. Partner Admin connects the **Business Central sandbox** (OAuth app registration) to a customer project. UNIFY reads metadata (companies, entities, fields, setup tables).
2. PM creates the project from a **playbook** (e.g., "BC Finance + Inventory — Standard"). The phases, tasks, migration objects and config templates are instantiated.
3. PM invites the customer. The customer uploads legacy extracts (Tally, QuickBooks, an old ERP, or Excel) through the portal.
4. UNIFY **profiles** each file: schema, types, completeness, duplicates, outliers, encoding.
5. The **Data Agent** proposes source→target mappings using the ERP schema, the playbook mapping templates and past approved mappings. Each mapping has a rationale and a confidence score.
6. The specialist reviews mappings (bulk-accepting high confidence, reviewing low) and adds transformation rules (value maps, lookups, splits, formulas) from a rule library.
7. **Deterministic validation** runs: mandatory fields, formats (GSTIN/VAT/IBAN), referential integrity (e.g., posting group exists), duplicates and business rules. Exceptions go to the owner (customer or consultant) with fix instructions.
8. The specialist approves the **load plan** (entity order, batch size, environment). The workflow executes the batches via the connector, idempotently, with retries.
9. **Reconciliation:** counts, key totals (opening balances by account, AR/AP totals) and sampled field-level comparisons. Variances block completion until explained.
10. **Configuration:** UNIFY applies the partner's config template, with values proposed from the approved requirements/questionnaire, executes after approval, reads back and diffs.
11. The **evidence pack** (migration report, reconciliation, config workbook, approvals) is generated for the customer's sign-off and feeds go-live readiness.

## 5. Functional requirements

Priority: **P0** = MVP, **P1** = MVP+ / GA, **P2** = later.

### 5.1 Platform and tenancy
| ID | Requirement | P |
|---|---|---|
| PLT-1 | Self-serve and assisted tenant sign-up; tenant types: partner, vendor | P0 |
| PLT-2 | Org hierarchy: Tenant → Customer (client account) → Project → Environment (sandbox/prod) | P0 |
| PLT-3 | RBAC with custom roles; permissions at tenant, customer, project, environment and action level; enforced in the backend | P0 |
| PLT-4 | External (customer) users scoped to their own customer/projects only | P0 |
| PLT-5 | SSO (OIDC/SAML: Entra ID, Google), MFA | P0 (MFA) / P1 (SAML) |
| PLT-6 | Plans, entitlements, usage metering, billing (Stripe/Razorpay) | P1 |
| PLT-7 | Notifications: in-app, email; Teams/Slack later | P0 / P2 |
| PLT-8 | White-label customer portal (logo, colours, subdomain) | P1 |

### 5.2 ERP adapters and knowledge
| ID | Requirement | P |
|---|---|---|
| ERP-1 | Register an ERP connection per project environment; credentials stored in the vault | P0 |
| ERP-2 | Metadata discovery: entities, fields, types, mandatory fields, relationships, setup tables | P0 |
| ERP-3 | Capability registry: only allow-listed operations per adapter version | P0 |
| ERP-4 | Adapter #1: **Dynamics 365 Business Central** (API v2.0 + config packages); **mock ERP**; **Odoo** reference adapter | P0 |
| ERP-5 | ERP knowledge pack: entity docs, setup dependencies and load order, standard validation rules, localisations (India GST, UAE VAT, UK VAT) | P0 (BC India/UK) |
| ERP-6 | Adapter SDK for new ERPs and for vendors | P1 |

### 5.3 Projects, playbooks and governance
| ID | Requirement | P |
|---|---|---|
| PRJ-1 | Playbooks: versioned templates of phases, tasks, migration objects, config templates, test packs, readiness gates | P0 |
| PRJ-2 | Project workspace: stage, milestones, tasks, owners, blockers, approvals, activity | P0 |
| PRJ-3 | Approvals engine: policies by action type, environment and materiality; multi-step; e-signature-style record | P0 |
| PRJ-4 | Exception queue: assign to internal or customer users, SLA, comments, attachments | P0 |
| PRJ-5 | Append-only audit log; exportable | P0 |
| PRJ-6 | Internal Control Tower dashboard (portfolio health, risks, capacity) | P1 |
| PRJ-7 | Customer Delivery Workspace (milestones, my actions, readiness, documents) | P0 (actions, uploads, approvals) / P1 (full) |
| PRJ-8 | Integrations: Jira/Azure DevOps/Rocketlane task sync, MS Teams | P2 |

### 5.4 Data migration factory
| ID | Requirement | P |
|---|---|---|
| MIG-1 | Upload Excel/CSV (multi-sheet, up to 1 M rows per file); preserve the original with a checksum; malware scan | P0 |
| MIG-2 | Profiling: types, null %, distinct counts, patterns, duplicates (exact and fuzzy), outliers | P0 |
| MIG-3 | AI mapping suggestions with rationale and confidence; learns from approved mappings within the tenant | P0 |
| MIG-4 | Transformation rule library: value maps, lookups, concat/split, date/number parsing, defaulting, code generation; versioned | P0 |
| MIG-5 | Deterministic validation engine: mandatory, format, reference, uniqueness, cross-field, balance rules | P0 |
| MIG-6 | Exception workflow with fix-in-portal or re-upload; Excel round-trip of exceptions | P0 |
| MIG-7 | Load plan: dependency-ordered entities, batches, dry-run, idempotency keys, retry, rollback where the ERP supports deletes in sandbox | P0 |
| MIG-8 | Reconciliation: counts, financial totals, sample field-level checks; variance sign-off | P0 |
| MIG-9 | Record-level lineage: source row → transformed record → target ID | P0 |
| MIG-10 | Entities: customers, vendors, items, chart of accounts, dimensions, opening balances (GL); then open AR/AP, inventory on hand, fixed assets | P0 / P1 |
| MIG-11 | Source connectors: Tally export parser, QuickBooks Online API, generic DB/ODBC extract via agent | P1 / P2 |
| MIG-12 | Mock and cutover rehearsals: re-run the full migration on a new data drop with the same approved rules; delta report | P1 |

### 5.5 Configuration
| ID | Requirement | P |
|---|---|---|
| CFG-1 | Configuration templates (partner IP): setup values, number series, posting groups, payment terms, tax setup, dimensions | P0 |
| CFG-2 | AI proposes values from the questionnaire and requirements; flags gaps | P0 |
| CFG-3 | Dependency check against the knowledge pack; execute via API/config package after approval | P0 |
| CFG-4 | Read-back verification and diff; configuration workbook export | P0 |
| CFG-5 | Configuration drift detection between sandbox and production | P2 |

### 5.6 Testing and readiness
| ID | Requirement | P |
|---|---|---|
| TST-1 | Generate test scenarios from requirements, config and playbook test packs (order-to-cash, procure-to-pay, record-to-report) | P1 |
| TST-2 | API-level automated execution of supported scenarios (e.g., post a sales invoice and verify GL entries) | P1 |
| TST-3 | UAT management for customer key users: assign, execute, evidence, defects | P1 |
| RDY-1 | Go-live readiness gates (as in the customer dashboard mock-up) computed from evidence; hard stops | P1 |
| RDY-2 | Cutover runbook with tasks, timings, owners and rollback | P2 |

### 5.7 Discovery and planning
| ID | Requirement | P |
|---|---|---|
| DSC-1 | Requirements questionnaire (playbook-driven) for the customer to fill in the portal | P0 (simple) |
| DSC-2 | AI extraction of requirements from documents/workshop notes/RFPs with source links | P1 |
| DSC-3 | Fit-gap against the ERP knowledge pack (standard / config / extension / gap) | P1 |
| DSC-4 | Plan generation from playbook + scope + complexity; effort estimate | P1 |
| DSC-5 | Proposal/SoW estimation support for presales | P2 |

### 5.8 Hypercare
| ID | Requirement | P |
|---|---|---|
| HYP-1 | Issue intake, classification, knowledge-based suggestions | P2 |

## 6. Non-functional requirements

| Area | Requirement |
|---|---|
| Security | See [09-security-compliance.md](09-security-compliance.md); tenant isolation tested every release |
| Availability | 99.5% (MVP), 99.9% (GA) for the web/API; workflows resume after any outage without data loss |
| Performance | Profile 100k rows in < 60 s; mapping suggestions for 50 columns in < 20 s; loads limited by ERP API throttling, with progress shown |
| Scalability | 500 concurrent projects, 50 M records/month by GA |
| Data residency | India and EU/UK regions by GA; US region later |
| Observability | Traces per workflow and agent run; cost per tenant/project |
| Accessibility | WCAG 2.1 AA for the portals |
| Localisation | English first; multi-currency and multi-date formats from day 1 |
| AI quality | Mapping top-1 accuracy ≥ 85% on the eval set before GA; no hallucinated fields (registry-enforced) |

## 7. Success metrics (product KPIs)

North Star: **implementation hours saved per active project per month.**

Supporting KPIs: automation coverage (eligible tasks done by UNIFY), mapping acceptance rate, first-pass validation rate, exception turnaround time, reconciliation pass on first run, configuration first-time-right, projects live on time, cost per implementation (from the blueprint), weekly active consultants, customer-portal adoption.

## 8. Release criteria (MVP)

All of the blueprint's acceptance tests TC-001…TC-017, re-pointed to the Business Central sandbox, plus:
- A real design-partner project migrates ≥ 5 entities with 100% lineage and reconciliation sign-off
- A security pen test is passed with no high or critical findings open
- A baseline vs. actual hours report shows ≥ 30% saving on the migration workstream
