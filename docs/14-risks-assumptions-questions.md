# 14 — Risks, Assumptions & Open Questions

## 1. Risk register (blueprint section 26, extended)

| # | Risk | L | I | Mitigation | Owner |
|---|---|---|---|---|---|
| R1 | Limited product API access (setup/config not exposed) | M | H | Phase 0 API spike; API readiness checklist; bulk-import fallback; co-develop missing APIs with the design partner | Tech lead |
| R2 | Each new product needs a custom adapter, so UNIFY becomes a services company | H | H | Strict contract; reuse patterns; SDK; charge adapter onboarding; target ≤ 4 weeks per product by GA | Product owners / tech lead |
| R3 | The product company decides to build in-house | M | H | Speed and depth; show the cost of building migration + governance + portal + AI evals; land before they start | Product owners |
| R4 | Long enterprise sales cycles / CTO security approval | M | H | Security pack early; sandbox-first; read-only start; SOC 2 roadmap | Product owners |
| R5 | Inconsistent customer data → AI misreads business meaning | M | H | Deterministic rules, exception queue, traceability, specialist approval (blueprint) | AI lead |
| R6 | Unsafe agent execution writes wrong data | L | Very H | Allow-listed tools, least privilege, sandbox-first, approvals, read-back, audit | Tech lead |
| R7 | Tenant data leakage | L | Very H | RLS, isolation tests, pen tests, vault | Tech lead |
| R8 | Scope expansion before proof | H | H | One design partner, limited entities, gate-based roadmap | Product owners |
| R9 | Unproven willingness to pay | M | H | Baseline + pilot commitments + paid conversion terms in the LOI | Product owners |
| R10 | Onboarding tools (Rocketlane, GUIDEcx) add execution features | M | M | Go deep on finance-grade migration, configuration and reconciliation | Product owners |
| R11 | Dependence on a single design partner | M | M | Recruit 2; keep the Odoo reference adapter as a demo product | Product owners |
| R12 | Trademark conflict on "Unify" | M | M | Early search; fallback name | Product owners |
| R13 | Hiring a strong tech lead takes too long | M | H | Start week 1; consider a technical co-founder | Product owners |

## 2. Assumptions to validate

The blueprint's five assumptions (section 27), plus three more:

| # | Assumption | How to validate | Phase |
|---|---|---|---|
| A1 | The first ERP product company will provide API documentation and sandbox access | Design-partner LOI; inspect API coverage, rate limits, auth, test data, write operations | 0 |
| A2 | The product company will standardise at least some implementation workflows | Interview implementation leaders; map repeated tasks; agree playbooks and validation rules | 0 |
| A3 | Customer master data is available in Excel/CSV (or parsable exports) | Review representative anonymised files | 0 |
| A4 | Product configuration operations can be exposed through supported APIs | API spike on the sandbox: read/write, permissions, read-back | 0 |
| A5 | There is a measurable financial incentive to adopt a separate implementation platform | Baseline hours/cost, rework and cycle time; willingness to pay; pilot conversion | 0 / 4 |
| A6 | Data migration + configuration are ≥ 30% of implementation effort for the ICP | Collect implementation hour breakdowns | 0 |
| A7 | The product company's customers will use a portal to fix their own data and approve | Prototype tests with 3–5 end customers | 0 / 2 |
| A8 | A new product's adapter can be built in ≤ 8 weeks (≤ 4 by GA) | Track actual effort on the design partner + Odoo adapters | 1–5 |

## 3. Open questions for the product owners

1. ~~**Customer type.**~~ **Answered 1 Nov 2026: ERP product companies, ERP first, other B2B products later.**
2. **Design partner.** Which ERP product company could be design partner #1? Which product is it, roughly how many implementations a year does it do, and does it have APIs and sandboxes?
3. **Roles and skills.** How will product, implementation-domain, technology and sales responsibilities be split between the product owners? This decides the first hires (e.g., whether a technical co-founder or tech lead is needed first).
4. **Domain knowledge.** Who will author the first playbooks and knowledge packs with the design partner's implementation team?
5. **Geography.** Is the first market India? Any specific regions or industries (distribution, manufacturing, pharma, retail…)?
6. **Team and funding.** Will Phase 0 be self-funded? Is the blueprint's ₹36–58 L budget fixed, or can it move to ~₹67 L–1 Cr for a stronger MVP (see [13](13-team-budget.md))?
7. **Company and IP.** Is there an existing company? How will ownership of the code, the blueprint and the brand be held?
8. **AI providers.** Any preferences or constraints (e.g., Azure OpenAI required by some customers, data residency for models)?
9. **Brand.** Committed to "UNIFY", or open to a variant if the trademark search shows conflicts?
10. **Dashboards.** Should the two HTML mock-ups be the target UX for MVP? (The plan puts their core in MVP and the full versions at GA.)
