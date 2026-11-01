# 10 — Roadmap & Release Plan

The blueprint asked stakeholders to approve **a time-boxed discovery and proof of concept, not the full build**. This roadmap keeps that gate and re-sequences the build around the wedge.

Start date assumed: **2 Nov 2026**. Rebaseline after Phase 0.

## Overview

| Phase | Weeks | Dates (indicative) | Outcome | Gate |
|---|---|---|---|---|
| **0. Validate & prove** | 0–8 | Nov – Dec 2026 | 15+ interviews, 2 design partners signed, technical spike on the real ERP sandbox, clickable prototype | **G0: go / pivot / stop** |
| **1. Foundation** | 9–14 | Jan – mid-Feb 2027 | Tenancy, auth, RBAC, projects, files, audit, mock + Odoo adapters | G1: isolation + mock E2E |
| **2. Migration factory** | 15–24 | mid-Feb – Apr 2027 | Profile → map → validate → load → reconcile on the BC sandbox; customer portal for uploads/exceptions | **G2: first real design-partner migration** |
| **3. Configuration + evidence** | 25–30 | May – mid-Jun 2027 | Config templates, apply + verify, evidence pack, approvals hardening | G3: E2E acceptance tests pass |
| **4. Pilot & MVP launch** | 31–36 | mid-Jun – Jul 2027 | 2–3 real projects, baseline vs actual, pen test, pricing live | **G4: ≥ 30% saving; 2 paying customers** |
| **5. GA + expansion** | 37–60 | Aug 2027 – Jan 2028 | Testing & readiness, discovery/fit-gap, Tally/QBO sources, adapter #2, SOC 2 Type I, UK region | G5: 10+ paying partners |
| **6. Platform** | 2028 | | Enterprise edition for large SIs (private deployment), partner pack marketplace, hypercare, adapters #3+ | |

## Phase 0 — Validate & prove (8 weeks) · detailed

| Week | Product / business | Technical |
|---|---|---|
| 1 | Finalise ICP and interview list (30–40 implementation partners); trademark check; register company and domain | Repo scaffold, CI, local stack (Postgres, Temporal, MinIO, Odoo docker); ADRs 001–009 |
| 2–3 | 8–10 interviews; collect anonymised sample files and setup checklists | BC sandbox spike: auth, metadata, create customers/suppliers/items, configuration packages, throttling |
| 4 | Synthesise; pick ERP #1 definitively; draft LOI | Mapping prototype: 50 real sample files → BC schema; measure accuracy across 2–3 models |
| 5–6 | Clickable Figma prototype of migration workbench + customer portal; demo to interviewees | Thin E2E spike: CSV → mapping → validation → BC sandbox load → count reconciliation (CLI quality) |
| 7 | Sign **2 design-partner LOIs** (pilot, data access, baseline measurement) | Eval set v0; architecture rebaseline |
| 8 | **G0 review**: evidence vs. the 5 blueprint assumptions; fundraising/budget decision | Sprint plan for Phases 1–4 |

**G0 pass criteria:** ≥ 2 signed design partners with sandbox access; mapping prototype ≥ 75% top-1 on real files; BC write path proven for 3 entities + 1 config area; ≥ 60% of interviewees rate the migration pain ≥ 4/5 and indicate a budget at the hypothesised price.

## Delivery cadence

- Two-week sprints; a demo to design partners every sprint from Phase 2
- Monthly steering review (founder, tech lead, design-partner champions)
- Release trains: staging weekly, production bi-weekly; feature flags per tenant

## Milestone checklist mapped to blueprint acceptance tests

| Blueprint TC | Phase |
|---|---|
| TC-001/002/003 tenant, product, mock connector | 1 |
| TC-015/017 unauthorised & cross-tenant access | 1 (and every release) |
| TC-004 onboard customer/project | 1 |
| TC-007/008/009 upload, validate, map | 2 |
| TC-010/011/016 migrate, reconcile, idempotent retry | 2 |
| TC-012 configuration | 3 |
| TC-005/006 requirements, plan | 5 (basic questionnaire in 2) |
| TC-013/014 tests, readiness | 5 |
