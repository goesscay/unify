# 10 — Roadmap & Release Plan

The UNIFY blueprint asked stakeholders to approve **a time-boxed discovery and technical proof of concept, not an unconditional commitment to the full build**. This roadmap keeps that gate, keeps the blueprint's delivery gates, and sequences the build around the wedge.

Start date assumed: **2 Nov 2026**. Rebaseline after Phase 0.

## Overview

| Phase | Weeks | Dates (indicative) | Outcome | Gate |
|---|---|---|---|---|
| **0. Validate & prove** | 0–8 | Nov – Dec 2026 | 12–15 interviews with ERP product companies; 1–2 design partners signed; API spike on the design partner's product; clickable prototype | **G0: go / revise / stop** |
| **1. Foundation** | 9–14 | Jan – mid-Feb 2027 | Tenancy, identity, roles, product registry, adapter contract, mock ERP + Odoo reference adapter, audit | **G1** (blueprint Gate 1): tenant isolation, product registry, mock connector demonstrated |
| **2. Design-partner adapter + migration** | 15–24 | mid-Feb – Apr 2027 | Design partner's product adapter certified; profile → map → validate → load → reconcile; customer portal for uploads and exceptions | **G2** (blueprint Gate 3): sandbox migration and reconciliation on the real product |
| **3. Configuration, governance, dashboards** | 25–30 | May – mid-Jun 2027 | Config templates, apply + verify, approvals hardening, evidence pack, Delivery Control Tower and Customer Delivery Workspace (core) | **G3** (blueprint Gate 4): E2E controlled workflow passes acceptance tests |
| **4. Pilot & MVP launch** | 31–36 | mid-Jun – Jul 2027 | 2–3 real customer implementations of the design partner; baseline vs actual; pen test; pricing live | **G4** (blueprint Gate 5): pilot evidence supports go / revise / stop; first paid contract |
| **5. GA + expansion** | 37–60 | Aug 2027 – Jan 2028 | Testing & readiness, discovery/fit-gap and plan generation, integrations, Tally/QuickBooks parsers, product companies #2–5, adapter onboarding ≤ 4 weeks, SOC 2 Type I | G5: 5+ paying product companies |
| **6. Platform** | 2028 | | Self-serve adapter SDK and low-code builder, embedded "Powered by UNIFY" migration, hypercare, first non-ERP category (e.g., WMS/logistics) | |

## Phase 0 — Validate & prove (8 weeks) · detailed

| Week | Product / business | Technical |
|---|---|---|
| 1 | Final ICP; list of 40 ERP product companies (India first, then GCC/SEA); trademark check; company/domain | Repo scaffold, CI, local stack (Postgres, Temporal, MinIO, Odoo docker), mock ERP v0; ADRs |
| 2–3 | 8–10 interviews (CEO, VP Implementation, CTO); collect implementation hour breakdowns, migration templates and anonymised sample files | Adapter contract v1; Odoo reference spike (metadata, create/update masters) |
| 4 | Synthesise; shortlist design partners; draft LOI | Mapping prototype: 50 real sample files → Odoo/mock schema; compare 2–3 LLMs |
| 5–6 | Clickable prototype (migration workbench, Control Tower, Customer Workspace) demoed to prospects | **API spike on the design partner's product sandbox**: auth, metadata, 3 entities, 1 setup area, limits |
| 7 | Sign **1–2 design-partner LOIs** (sandbox/API access, 2–3 customer implementations, baseline data) | Thin E2E: CSV → mapping → validation → design partner sandbox load → count reconciliation |
| 8 | **G0 review** against the blueprint's five assumptions; budget/fundraising decision | Rebaselined sprint plan |

**G0 pass criteria** (blueprint section 27, made measurable):
1. API documentation and sandbox access received from ≥ 1 design partner
2. The design partner agrees to standardise at least the migration and core setup workflow as a playbook
3. Representative customer files are in Excel/CSV (or parsable exports)
4. Write operations for ≥ 3 entities and ≥ 1 setup area are proven via API
5. A measurable incentive: baseline hours/cost collected, and willingness to pay at the hypothesised price range

## Delivery cadence

- Two-week sprints; sprint demos to the design partner from Phase 2
- Monthly steering (product owners, tech lead, the design partner's implementation head)
- Staging weekly, production bi-weekly; feature flags per tenant

## Blueprint acceptance tests by phase

| Blueprint TC | Phase |
|---|---|
| TC-001 register vendor (product company), TC-002 register product, TC-003 connect mock ERP | 1 |
| TC-015 unauthorised execution, TC-017 cross-tenant access | 1 (and every release) |
| TC-004 onboard customer | 1 |
| TC-007 upload, TC-008 validate, TC-009 map | 2 |
| TC-010 migrate, TC-011 reconcile, TC-016 retry without duplicates | 2 |
| TC-012 configuration | 3 |
| TC-005 requirements, TC-006 plan | 5 (questionnaire in 2) |
| TC-013 test execution, TC-014 readiness | 5 (readiness view basic in 3) |
