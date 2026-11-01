# 04 — Product Requirements Document (PRD)

Version 0.3 · builds on Part A of the UNIFY blueprint, re-sequenced around the wedge in [02-product-strategy.md](02-product-strategy.md). **Customers are ERP product companies** that implement their own product for their customers.

## 1. Problem statement

ERP product companies rely on implementation consultants to gather requirements, create plans, chase spreadsheets, cleanse and map data, configure modules, coordinate integrations, test scenarios and prepare go-live. These activities recur for every customer but are executed manually and inconsistently. That delays revenue, caps growth at consultant headcount and hurts customer satisfaction. UNIFY standardises and automates the eligible repeatable work, while humans keep the judgement on exceptions and material decisions.

## 2. Goals and non-goals

**Goals (MVP)**
- G1. Connect the design partner's ERP product through a certified adapter in **≤ 8 weeks**.
- G2. Cut implementation effort on customer data migration and standard configuration by **≥ 30%**, measured against a baseline.
- G3. Reduce elapsed time from "customer data received" to "approved sandbox migration" by **≥ 40%**.
- G4. **100%** of migrated records traceable to the source row or an explicit exception; zero unauthorised writes.
- G5. The design partner completes **2–3 real customer implementations** through UNIFY and converts to a paid contract.

**Non-goals (MVP)** (from the blueprint's out-of-scope list): universal support for every ERP; fully autonomous production go-live; complex legacy UI automation; extensive banking/payment connector library; a complete ERP application; replacing CRM/project-management tools; autonomous financial approvals; a full support-ticketing product.

## 3. Personas and roles

| Persona | Organisation | Responsibilities | Key screens |
|---|---|---|---|
| **UNIFY Platform Super Admin** | UNIFY | Platform operations, tenant lifecycle, global policies, adapter certification | Admin console |
| **Product Company Admin** | ERP product company | Register product, versions, modules, adapter/connector, environments, users, playbooks, templates, billing | Products, Connectors, Playbooks, Users, Settings |
| **Product / API Owner** (CTO or product lead) | ERP product company | Approve API scopes, review capability registry, security | Connectors, Capability registry, Audit |
| **Head of Implementation / PMO** | ERP product company | Portfolio health, capacity, go-live predictability | Delivery Control Tower (mock-up 1) |
| **Implementation Manager** | ERP product company | Scope, plan, milestones, risks, approvals, go-live readiness | Project workspace, Approvals, Readiness |
| **Implementation Consultant** | ERP product company | Requirements, configuration, exception resolution | Configuration, Requirements, Exceptions |
| **Data Migration Specialist** | ERP product company | Mappings, rules, migration runs, reconciliation | Migration workbench |
| **QA Specialist** | ERP product company | Test scenarios, execution, defects, evidence | Testing |
| **Customer Admin / Sponsor** | The product company's customer | Submit requirements and data, approvals, go/no-go | Customer Delivery Workspace (mock-up 2) |
| **Customer Business User / Data Owner** | The product company's customer | Fix data exceptions, UAT, sign-off | Customer action centre, exception queue, UAT |

Permissions are configurable at tenant, product, customer, project, environment and action level, and enforced in the backend (blueprint section 5).

## 4. Core user journeys

### 4.1 Product onboarding (once per product)
1. The Product Company Admin registers the product: name, versions, modules, hosting model, regions.
2. Connect a **reference sandbox** of the product. The adapter discovers metadata: entities, fields, types, mandatory fields, relationships, setup tables.
3. Review and publish the **capability registry**: which operations UNIFY may perform (create customers, update items, apply setup X…), per environment.
4. Import or author the **knowledge pack**: implementation rules, entity load order, standard validations, localisations, configuration templates, test packs, and the product company's existing migration templates.
5. Define **playbooks** (e.g., "Distribution – Standard", "Manufacturing – Advanced").
6. Run the adapter **conformance suite**; UNIFY certifies the adapter.

### 4.2 Customer implementation (every customer)
1. The Implementation Manager creates the customer and project from a playbook; phases, tasks, migration objects, config templates and readiness gates are instantiated.
2. The customer's sandbox (and later production) environment is linked; credentials go to the vault.
3. The Customer Admin is invited to the **Customer Delivery Workspace** and completes the questionnaire and data uploads.
4. **Migration:** profile → AI mapping (rationale + confidence) → specialist review → deterministic validation → exceptions to the customer → load-plan approval → batched, idempotent load to sandbox → reconciliation → sign-off.
5. **Configuration:** config template + AI-proposed values from the questionnaire → diff → approval → execute → read-back verification.
6. **Evidence pack** and readiness view are updated automatically. The customer sponsor makes the go/no-go decision.

## 5. Functional requirements

Priority: **P0** = MVP, **P1** = GA, **P2** = later.

### 5.1 Platform and tenancy
| ID | Requirement | P |
|---|---|---|
| PLT-1 | Tenant = ERP product company; assisted onboarding with UNIFY approval before activation | P0 |
| PLT-2 | Hierarchy: Tenant → Products → Customers → Projects → Environments | P0 |
| PLT-3 | RBAC with custom roles; scoped assignments (tenant, product, customer, project, environment, action); backend enforced | P0 |
| PLT-4 | Customer users scoped strictly to their own projects | P0 |
| PLT-5 | MFA (P0); SSO via OIDC/SAML and SCIM (P1) | P0/P1 |
| PLT-6 | Plans, entitlements, metering, billing | P1 |
| PLT-7 | Notifications: in-app, email (P0); Teams/Slack (P2) | P0/P2 |
| PLT-8 | White-label customer portal in the product company's brand (logo, colours, custom domain) | P1 |

### 5.2 Product registry and adapters
| ID | Requirement | P |
|---|---|---|
| PRD-1 | Register product, versions, modules, entities, fields, capabilities, validation rules, environments (blueprint Module 01) | P0 |
| PRD-2 | Adapter contract implementation for the design partner's product; mock-ERP adapter; reference adapter (Odoo) for demos and CI | P0 |
| PRD-3 | Metadata discovery from the product's API (OpenAPI/OData $metadata import where available) | P0 |
| PRD-4 | Capability registry with versioning, per-environment permissions and an approval workflow by the Product/API Owner | P0 |
| PRD-5 | Knowledge pack import (Excel/YAML/docs); retrieval index | P0 |
| PRD-6 | Adapter conformance suite and certification | P0 |
| PRD-7 | Self-serve adapter SDK + low-code adapter builder (map REST endpoints to contract operations) | P1/P2 |
| PRD-8 | Product version upgrades: diff capabilities between versions; flag affected playbooks and mappings | P1 |

### 5.3 Customers, projects, playbooks, governance
| ID | Requirement | P |
|---|---|---|
| PRJ-1 | Customer onboarding profile: company, industry, legal entities, branches, modules, users, source systems, target dates, documents (blueprint Module 02) | P0 |
| PRJ-2 | Versioned playbooks: phases, tasks, migration objects, config templates, test packs, readiness gates | P0 |
| PRJ-3 | Project workspace: stage, milestones, tasks, dependencies, owners, blockers, approvals, activity | P0 |
| PRJ-4 | Approvals engine: policies by action type, environment, materiality; multi-step; segregation of duties | P0 |
| PRJ-5 | Exception queue for internal and customer owners, with SLA | P0 |
| PRJ-6 | Append-only, hash-chained audit log; export | P0 |
| PRJ-7 | **Delivery Control Tower** (mock-up 1): portfolio health, stage distribution, risks/decisions, capacity, governance | P0 (core KPIs) / P1 (full) |
| PRJ-8 | **Customer Delivery Workspace** (mock-up 2): summary, milestones, my actions, readiness gates, risks/decisions, documents & support | P0 (actions, uploads, approvals, milestones) / P1 (full) |
| PRJ-9 | Sync with the product company's CRM/PSA (Salesforce/HubSpot deal → project; Jira; Rocketlane) | P2 |

### 5.4 Data migration (blueprint Module 05)
| ID | Requirement | P |
|---|---|---|
| MIG-1 | Upload Excel/CSV (multi-sheet, up to 1 M rows); preserve the original + checksum; malware scan | P0 |
| MIG-2 | Profiling: types, nulls, distincts, patterns, duplicates (exact/fuzzy), outliers; downloadable report | P0 |
| MIG-3 | AI mapping suggestions with rationale and confidence, restricted to registry fields; learns from approved mappings for that product | P0 |
| MIG-4 | Transformation rule library: value maps, lookups, split/concat, date/number parsing, defaults, code generation; versioned | P0 |
| MIG-5 | Deterministic validation: mandatory, format (GSTIN/VAT/PAN/IBAN), reference, uniqueness, cross-field, balance rules | P0 |
| MIG-6 | Exceptions fixed by the customer in the portal or via Excel round-trip; re-validation | P0 |
| MIG-7 | Load plan: dependency order, batches, dry run, idempotency, retry; sandbox rollback where supported | P0 |
| MIG-8 | Reconciliation: counts, financial totals, sampled field checks; variance sign-off | P0 |
| MIG-9 | Record-level lineage: source row → transformed record → target ID | P0 |
| MIG-10 | Entities: customers, suppliers, items, chart of accounts, branches/locations, opening balances (P0); open AR/AP, inventory, fixed assets (P1) | P0/P1 |
| MIG-11 | Source parsers: Tally, QuickBooks, common legacy ERP exports | P1 |
| MIG-12 | Mock-cutover rehearsals with delta reports; production migration with explicit two-person authorisation | P1 |

### 5.5 Configuration (blueprint Module 06)
| ID | Requirement | P |
|---|---|---|
| CFG-1 | Configuration templates per product/playbook | P0 |
| CFG-2 | AI proposes values from questionnaire and requirements; flags gaps | P0 |
| CFG-3 | Capability and dependency checks; execute via adapter after approval | P0 |
| CFG-4 | Read-back verification, before/after evidence, configuration workbook export | P0 |
| CFG-5 | Sandbox-to-production configuration promotion and drift detection | P2 |

### 5.6 Integration management (blueprint Module 07)
| ID | Requirement | P |
|---|---|---|
| INT-1 | Register external systems; vault-referenced credentials; REST/webhook connectivity tests | P1 |
| INT-2 | Field mapping, request/response validation, retry of recoverable failures, integration report | P1 |
| INT-3 | SFTP file exchange | P2 |

### 5.7 Testing and go-live (blueprint Module 08)
| ID | Requirement | P |
|---|---|---|
| TST-1 | Generate requirement-linked tests from requirements, configuration and test packs | P1 |
| TST-2 | Execute supported scenarios via the adapter; capture evidence; record defects; retest | P1 |
| TST-3 | UAT for customer business users | P1 |
| RDY-1 | Readiness gates computed from evidence; hard stops; customer go/no-go | P1 |
| RDY-2 | Cutover runbook and rollback plan | P2 |

### 5.8 Discovery and planning (blueprint Modules 03–04)
| ID | Requirement | P |
|---|---|---|
| DSC-1 | Playbook-driven customer questionnaire in the portal | P0 |
| DSC-2 | AI extraction of requirements from documents with source links; module classification | P1 |
| DSC-3 | Fit-gap against the product's capability registry and knowledge pack | P1 |
| DSC-4 | Plan generation (tasks, dependencies, owners, dates, risks) for manager approval | P1 |

### 5.9 Hypercare
| ID | Requirement | P |
|---|---|---|
| HYP-1 | Issue classification and knowledge-based resolution suggestions for human review | P2 |

## 6. Non-functional requirements

| Area | Requirement |
|---|---|
| Security | See [09-security-compliance.md](09-security-compliance.md); tenant-isolation tests every release |
| Availability | 99.5% (MVP), 99.9% (GA); workflows resume after outages without data loss |
| Performance | Profile 100k rows < 60 s; mapping suggestions for 50 columns < 20 s; loads bounded by product API limits |
| Scalability | 1,000 concurrent customer projects, 50 M records/month by GA |
| Data residency | India first; EU/UK by GA |
| Observability | Traces per workflow and agent run; cost per tenant/project |
| Accessibility | WCAG 2.1 AA for portals |
| AI quality | Mapping top-1 ≥ 85% on the product's eval set before GA; zero non-registry fields |

## 7. Success metrics (blueprint Part K)

**Most important: cost per successfully implemented customer** (from the blueprint), together with **time from kickoff to go-live**.

Supporting KPIs: automation coverage, human intervention rate, data migration accuracy, migration exception rate, first-time-right configuration, integration success rate, test pass rate, go-live readiness, customer action turnaround, adapter onboarding time (for UNIFY).

## 8. MVP release criteria

The blueprint's acceptance tests TC-001…TC-017 pass against the design partner's product sandbox, plus:
- The pilot exit criteria from the blueprint (traceability, authorisation, no cross-tenant leakage, reconciliation, evidenced tests, a single-dashboard view of blockers, ≥ 2 customer implementations completed)
- A pen test with no open high or critical findings
- A baseline vs. actual report showing ≥ 30% effort saving on migration and configuration
