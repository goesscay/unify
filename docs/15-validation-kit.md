# 15 — Validation Kit (Phase 0)

## 1. Interview targets

- 30–40 implementation partners (practice heads, delivery heads, migration leads) across 2–3 ERP ecosystems, including 3–5 practice leads inside large SIs
- 5 end customers who recently went live (sponsor, finance or data owner)

## 2. Partner interview script (45 min)

**Context (5 min)**
1. Which ERPs do you implement? How many projects a year? Typical size (hours, duration, price)? Fixed price or T&M?

**Current process (15 min)**
2. Walk me through your last implementation's data migration, from first customer file to sign-off.
3. Which tools and templates do you use? (Ask them to share an anonymised mapping sheet or setup checklist.)
4. How many hours did migration, configuration and testing take? Who did it (seniority)?
5. What went wrong? Where did the delays come from (customer data, mapping, reconciliation, rework)?
6. How do you capture what you learned for the next project?

**Pain and priority (10 min)**
7. Rate 1–5 the pain of: data migration, configuration, testing/UAT, requirements/fit-gap, customer follow-ups, go-live readiness.
8. If you could automate one thing tomorrow, what would it be?
9. Do you use AI today? How? What stops you using it more (trust, data security, client consent)?

**Solution and commercials (10 min)**
10. (Show the prototype.) What would you need to see to trust this with a customer's data?
11. Would your customers use a portal to fix their own data? Would they accept a cloud tool processing their data?
12. How do you buy tools today? Who decides? Budget range?
13. Pricing test: "If this saved ~30% of migration and configuration effort, would ~US$X per project or ~US$Y per month be reasonable?" (Use the Van Westendorp price questions.)

**Close (5 min)**
14. Would you pilot this on a real upcoming project in the next 3 months, with sandbox access and a baseline?

## 3. Scoring

For each interview record: ERP, firm size, project volume, pain scores, current hours, AI attitude, price reaction, pilot willingness (Y/N/maybe). **Signal threshold for G0:** ≥ 60% rate migration pain ≥ 4, ≥ 30% open to a pilot, and 2 signed LOIs.

## 4. Design-partner LOI — key terms

- Pilot duration 90 days; 2–3 real projects; ERP sandbox access; anonymised or consented data
- Partner provides a baseline (hours and timeline for comparable past projects) and a champion (≥ 4 h/week)
- UNIFY provides a free pilot, white-glove onboarding, priority roadmap input
- Success criteria agreed upfront (below); if met, an annual contract at the design-partner price (50% off list in year 1)
- Mutual NDA + DPA; the customer's consent for data processing; UNIFY may use anonymised learnings and the case study with approval

## 5. Technical proof-of-concept success criteria

| Criterion | Target |
|---|---|
| Entities migrated to sandbox | ≥ 3 (customers, vendors, items) |
| Mapping top-1 accuracy on partner files | ≥ 75% (PoC), ≥ 85% (MVP) |
| Lineage | 100% records traceable or in exceptions |
| Duplicate creation on retry | 0 |
| Reconciliation | Counts match; variances explained |
| Configuration | ≥ 1 setup area applied and verified via API |
| Effort | Migration workstream hours ≥ 30% below baseline |
