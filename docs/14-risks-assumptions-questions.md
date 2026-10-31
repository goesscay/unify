# 14 — Risks, Assumptions & Open Questions

## 1. Risk register

| # | Risk | L | I | Mitigation | Owner |
|---|---|---|---|---|---|
| R1 | ERP vendors ship native AI implementation tooling (SAP already has) | H | H | Mid-market ERPs first; cross-ERP value; list on vendor marketplaces; partner with vendors | Founder |
| R2 | Fast-moving startup competitor (superglue, Tato) captures the partner segment | M | H | Narrow + deep wedge; governance + customer portal; speed to design partners | Founder |
| R3 | ERP API gaps for setup/config | M | H | Phase 0 spike; use bulk/config-package import; scope config to API-supported areas | Tech lead |
| R4 | AI mapping quality insufficient on messy real data | M | H | Deterministic rules first; mapping memory; eval gold set; confidence thresholds; human review | AI lead |
| R5 | Partners resist automation that reduces billable hours | M | M | Sell margin on fixed price + capacity; per-project pricing they can pass through | Founder |
| R6 | Data security incident | L | Very H | Controls in [09](09-security-compliance.md); pen tests; least privilege; sandbox-first | Tech lead |
| R7 | Scope creep toward "universal" before proof | H | H | Gate-based roadmap; one ERP; P0 discipline | Founder |
| R8 | Services trap (custom adapters for each client) | M | M | Cap services at <20% of revenue; SDK; packs over custom code | Founder |
| R9 | LLM cost erodes margin | L | M | Profiles not datasets; small models; caching; budgets | AI lead |
| R10 | Trademark conflict on "Unify" | M | M | Early search; fallback name | Founder |
| R11 | Hiring a strong tech lead takes too long | M | H | Start recruiting in week 1; consider a technical co-founder with equity | Founder |

## 2. Assumptions to validate (blueprint's five + new)

| # | Assumption | Validation method | Phase |
|---|---|---|---|
| A1 | Design partners will provide ERP sandbox access and anonymised data | LOIs | 0 |
| A2 | Partners have repeatable workflows they will standardise into playbooks | Interviews; collect their templates | 0 |
| A3 | Source data mostly arrives as Excel/CSV (or Tally/QBO exports) | Sample-file review | 0 |
| A4 | Key configuration areas are API-accessible on ERP #1 | Technical spike | 0 |
| A5 | Measurable financial incentive and willingness to pay at the hypothesised price | Pricing questions, LOIs, pilot conversion | 0 / 4 |
| A6 | **Implementation partners (not only vendors) are the better first buyer** | Compare interview signals from both groups | 0 |
| A7 | Migration + configuration is ≥ 25% of project effort for the ICP | Collect project hour breakdowns | 0 |
| A8 | Customers will use a portal to fix their own data exceptions | Prototype tests with end customers | 0 / 2 |
| A9 | ≥ 85% mapping accuracy is achievable on real files | Eval gold set | 0 / 2 |

## 3. Open questions for the founder

Answers to these will change the plan. Please respond when ready.

1. **First customer type.** Should we target implementation partners/SIs first (recommended), ERP software vendors first (the blueprint), or both from day one?
2. **First ERP.** Which ERP ecosystem does your network know best: Business Central, Odoo, SAP Business One, NetSuite, Tally/Zoho-centric Indian ERPs, or another? Do you have a specific ERP vendor or partner in mind as design partner #1?
3. **Your background.** Are you an ERP practitioner yourself (which ERPs and modules)? This decides who authors the first knowledge packs.
4. **Geography.** Is the first market India, GCC, UK/EU or US? This drives hosting region, localisation (GST/VAT) and pricing.
5. **Team and funding.** Are you funding Phase 0 yourself? Do you have a technical co-founder or tech lead? Is the ₹36–58 L budget fixed, or can it move to the ₹67 L–1 Cr range for a stronger MVP?
6. **Large SIs.** Is "major ERP implementation companies" meant as large global SIs (Accenture, Infosys, TCS…) in the near term? They need private deployment and long sales cycles, so the plan treats them as year 2+.
7. **Company IP and entity.** Is there an existing company, or will a new one be set up? Who owns the code? (This affects contractors and the GitHub org.)
8. **AI provider preferences or constraints** (e.g., customers requiring Azure OpenAI, a data-residency need for model hosting)?
9. **Brand.** Are you committed to the name "UNIFY", or open to a variant if the trademark search shows conflicts?
10. **Dashboards.** Should the two HTML mock-ups (internal Control Tower and Customer Delivery Workspace) be treated as the target UX for the MVP, or as later-phase (P1) screens? The plan currently puts the customer action centre in the MVP and the full Control Tower at GA.
