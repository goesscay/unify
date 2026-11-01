# 11 — Product Backlog

Re-prioritised from Part H of the blueprint for the wedge. P0 = MVP (Phases 1–4), P1 = GA (Phase 5), P2 = later. Stories use the format *As a … I want … so that …*, followed by acceptance criteria (AC).

## Epics

| Epic | Name | P | Phase |
|---|---|---|---|
| E01 | Multi-tenant foundation (tenants, users, RBAC, audit) | P0 | 1 |
| E02 | ERP catalogue, adapter SDK, capability registry | P0 | 1 |
| E03 | Mock + Odoo adapters | P0 | 1 |
| E04 | Business Central adapter + knowledge pack | P0 | 2 |
| E05 | Customers, projects, playbooks | P0 | 1 |
| E06 | Files, ingestion, profiling | P0 | 2 |
| E07 | AI mapping + mapping memory | P0 | 2 |
| E08 | Rules engine: transform + validate | P0 | 2 |
| E09 | Exceptions + customer portal | P0 | 2 |
| E10 | Load engine + reconciliation + lineage | P0 | 2 |
| E11 | Approvals + policy engine | P0 | 1–2 |
| E12 | Configuration templates + execution | P0 | 3 |
| E13 | Evidence pack + reports | P0 | 3 |
| E14 | AI gateway, eval harness, LLM observability | P0 | 1–2 |
| E15 | Billing, plans, metering | P1 | 4 |
| E16 | Internal Control Tower dashboard | P1 | 5 |
| E17 | Testing + UAT | P1 | 5 |
| E18 | Readiness gates + cutover | P1 | 5 |
| E19 | Discovery, fit-gap, plan generation | P1 | 5 |
| E20 | Source connectors (Tally, QBO) | P1 | 5 |
| E21 | Adapter #2 | P1 | 5 |
| E22 | SSO/SAML, SCIM, data residency (UK) | P1 | 5 |
| E23 | Enterprise edition for large SIs (private deployment, multi-practice) | P2 | 6 |
| E24 | Hypercare | P2 | 6 |
| E25 | Marketplace for packs | P2 | 6 |

## Key P0 stories

**E01 Foundation**
- US-001 *As a partner admin I want to create my workspace so my team can start.* AC: tenant + admin created; region chosen; MFA enforced; audit event recorded.
- US-002 *As a partner admin I want to assign roles at tenant/customer/project scope.* AC: backend enforcement; denied actions return 403 and are audited; customer users never see other customers (automated test).
- US-003 *As a security reviewer I want an exportable audit log.* AC: append-only, hash-chained, filterable, CSV/JSON export.

**E02–E04 Adapters**
- US-010 *As an admin I want to connect a BC sandbox to a project environment.* AC: OAuth app flow; secret stored in vault; health check; metadata discovered; failures explained.
- US-011 *As an engineer I want the agent toolset generated from the capability registry.* AC: an unregistered operation cannot be called (test).
- US-012 *As an engineer I want an adapter conformance suite.* AC: mock, Odoo and BC pass; runs nightly.

**E05 Projects & playbooks**
- US-020 *As a PM I want to create a project from a playbook.* AC: phases, tasks, migration objects, config templates instantiated with a pinned playbook version.
- US-021 *As a PM I want to invite customer users with limited roles.* AC: the invite email arrives; scope is limited to that customer's project.

**E06–E08 Ingestion, mapping, rules**
- US-030 *As a customer data owner I want to upload Excel/CSV in the portal.* AC: ≤ 200 MB; malware scan; checksum; the original is immutable; sheets detected.
- US-031 *As a specialist I want a profile of each sheet.* AC: types, nulls, distincts, patterns, duplicates (exact + fuzzy), downloadable report.
- US-032 *As a specialist I want AI mapping suggestions with rationale and confidence.* AC: only registry fields; bulk-accept above the threshold; every mapping versioned with its origin.
- US-033 *As a specialist I want reusable transformation rules.* AC: value maps, lookups, split/concat, date/number parsing, defaults; preview on 100 rows; versioned.
- US-034 *As a specialist I want deterministic validation.* AC: mandatory, format (GSTIN, VAT, email, IBAN), references to ERP setup, uniqueness; results per record.

**E09 Exceptions & portal**
- US-040 *As a customer data owner I want to see and fix my exceptions.* AC: grouped by issue with fix guidance; inline edit or Excel round-trip; SLA due date; re-validation on save.

**E10 Load & reconcile**
- US-050 *As a specialist I want to run a dry run, then an approved load to the sandbox.* AC: load plan approval required; dependency order; batches; progress; idempotent retry creates no duplicates (TC-016).
- US-051 *As a PM I want reconciliation.* AC: counts, totals by account/customer, sampled field comparison; variances require sign-off; lineage source row → target ID for 100% of records.

**E11 Approvals**
- US-060 *As an admin I want approval policies per action and environment.* AC: policy evaluation recorded; the approver cannot be the requester when the policy requires; production writes blocked in MVP.

**E12–E13 Configuration & evidence**
- US-070 *As a consultant I want to apply a config template with AI-proposed values.* AC: diff vs. current; dependency order; approval; read-back verification; config workbook (Excel) export.
- US-071 *As a PM I want an evidence pack.* AC: PDF + ZIP with migration report, reconciliation, config workbook, approvals and audit excerpt; shareable with the customer.

**E14 AI platform**
- US-080 *As an engineer I want an eval harness in CI.* AC: the mapping gold set runs on each prompt/model change; release blocked on regression > 2 pts.
- US-081 *As an admin I want AI cost per project.* AC: tokens and cost per agent run, aggregated per tenant/project.

## Definition of Done (all stories)

Code reviewed · unit + integration tests · tenant-isolation tests updated · audit events for state changes · API documented (OpenAPI) · feature flag · observability (traces) · docs updated · accessibility check for UI.
