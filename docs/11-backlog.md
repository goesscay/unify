# 11 — Product Backlog

Re-prioritised from Part H of the SME blueprint for the wedge. P0 = MVP (Phases 1–4), P1 = GA (Phase 5), P2 = later. Stories use *As a … I want … so that …* followed by acceptance criteria (AC).

## Epics (blueprint EPIC-01…12, extended)

| Epic | Name | Blueprint | P | Phase |
|---|---|---|---|---|
| E01 | Multi-tenant platform foundation | EPIC-01 | P0 | 1 |
| E02 | Product registry (product, versions, modules, entities, fields) | EPIC-02 | P0 | 1 |
| E03 | Connector framework: adapter contract, capability registry, conformance suite | EPIC-03 | P0 | 1 |
| E04 | Mock ERP + Odoo reference adapter | EPIC-03 | P0 | 1 |
| E05 | Design partner's product adapter + knowledge pack | EPIC-03 | P0 | 2 |
| E06 | Customer onboarding, projects, playbooks | EPIC-04 | P0 | 1 |
| E07 | File ingestion and data profiling | EPIC-06 | P0 | 2 |
| E08 | AI data mapping + mapping memory | EPIC-07 | P0 | 2 |
| E09 | Rules engine: transform + validate | EPIC-07 | P0 | 2 |
| E10 | Exceptions + Customer Delivery Workspace (core) | — | P0 | 2–3 |
| E11 | Load engine, reconciliation, lineage | EPIC-07 | P0 | 2 |
| E12 | Approval, audit and governance | EPIC-11 | P0 | 1–2 |
| E13 | Configuration execution | EPIC-08 | P0 | 3 |
| E14 | Evidence pack + Delivery Control Tower (core) | — | P0 | 3 |
| E15 | AI gateway, eval harness, LLM observability | — | P0 | 1–2 |
| E16 | Billing, plans, metering | — | P1 | 4 |
| E17 | Testing and validation | EPIC-10 | P1 | 5 |
| E18 | Go-live readiness and cutover | EPIC-12 | P1 | 5 |
| E19 | AI discovery and planning | EPIC-05 | P1 | 5 |
| E20 | Integration framework (REST/webhooks/SFTP) | EPIC-09 | P1 | 5 |
| E21 | Source parsers (Tally, QuickBooks) | — | P1 | 5 |
| E22 | SSO/SAML, SCIM, white-label portal, EU/UK region | — | P1 | 5 |
| E23 | Self-serve adapter SDK + low-code adapter builder | — | P2 | 6 |
| E24 | Embedded "Powered by UNIFY" migration API | — | P2 | 6 |
| E25 | Hypercare | EPIC-12 | P2 | 6 |
| E26 | First non-ERP product category | — | P2 | 6 |

## Key P0 stories (blueprint US-001…016, refined)

**E01 Foundation**
- US-001 *Product company registration.* AC: tenant created in pending state; UNIFY admin approves; admin assigned; data isolated; MFA login works.
- US-002 *Role-based access.* AC: roles assignable at tenant/product/customer/project/environment scope; backend enforced; unauthorised actions rejected and audited (TC-015); cross-tenant access denied (TC-017).
- US-003 *Audit export.* AC: append-only, hash-chained, filterable, CSV/JSON export.

**E02–E05 Product and connector**
- US-010 *Register product.* AC: product, version, modules, entities, fields and capabilities stored; activation supported (TC-002).
- US-011 *Configure connector.* AC: credentials in vault; connection test; metadata and capabilities discovered; failures logged (TC-003).
- US-012 *Approve capability registry.* AC: the Product/API Owner approves operations per environment; the agent toolset reflects only approved capabilities (test).
- US-013 *Adapter conformance.* AC: mock, Odoo and the design partner's adapter pass the suite; runs nightly.

**E06 Customers and projects**
- US-020 *Create customer implementation.* AC: customer details and product captured; project created from a pinned playbook version; customer admin invited (TC-004).

**E07–E09 Ingestion, mapping, rules**
- US-030 *Upload master data.* AC: Excel/CSV accepted; invalid formats rejected; original preserved with checksum; malware scan; project association correct (TC-007).
- US-031 *Data profiling.* AC: counts, mandatory gaps, duplicates and anomalies flagged; report downloadable (TC-008).
- US-032 *AI field mapping.* AC: rationale and confidence shown; only registry fields; ambiguous mappings require review; versions retained (TC-009).
- US-033 *Transformation rules.* AC: value maps, lookups, split/concat, parsing, defaults; preview on 100 rows; versioned.
- US-034 *Deterministic validation.* AC: mandatory, format, reference, uniqueness, cross-field rules; per-record results.

**E10 Exceptions and customer workspace**
- US-040 *Customer fixes exceptions.* AC: grouped by issue with fix guidance; inline edit or Excel round-trip; due date; re-validation on save.
- US-041 *Customer action centre.* AC: the customer sees their own actions, owners, due dates, milestones and approvals only (mock-up 2).

**E11 Load and reconcile**
- US-050 *Execute migration.* AC: approved mappings only; load-plan approval; batches; failures captured; safe retries with no duplicates (TC-010, TC-016).
- US-051 *Reconcile.* AC: counts, financial totals, sampled field checks; variances require sign-off; lineage for 100% of records (TC-011).

**E12 Governance**
- US-060 *Approval policies.* AC: per action/environment/materiality; segregation of duties; production writes blocked in MVP.

**E13–E14 Configuration, evidence, Control Tower**
- US-070 *Configuration recommendation and execution.* AC: capabilities and dependencies checked; approval before execution; read-back verified; before/after evidence (TC-012).
- US-071 *Evidence pack.* AC: migration report, reconciliation, config workbook, approvals and audit excerpt; shareable with the customer.
- US-072 *Delivery Control Tower (core).* AC: active projects, health with reasons, stage distribution, pending approvals, open exceptions, critical risks (mock-up 1); all figures tenant-scoped.

**E15 AI platform**
- US-080 *Eval harness.* AC: the mapping gold set runs on each prompt/model change; release blocked on regression > 2 pts.
- US-081 *AI cost visibility.* AC: tokens and cost per agent run, per tenant/project.

## Definition of Done

Code reviewed · unit + integration tests · tenant-isolation tests updated · audit events for state changes · OpenAPI documented · feature flag · traces · docs updated · accessibility check for UI.
