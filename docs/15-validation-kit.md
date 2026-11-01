# 15 — Validation Kit (Phase 0)

Follows the blueprint's recommended validation sequence: 5–8 (here 12–15) structured interviews → select one design partner → review APIs and anonymised files → narrow technical proof → compare against baseline and test willingness to pay.

## 1. Interview targets

- 12–15 ERP product companies: CEO/founder, VP/Head of Implementation, CTO
- 3–5 implementation consultants or data-migration specialists inside those companies
- 3–5 of their customers who went live recently (sponsor, finance or data owner)

## 2. Interview script — Head of Implementation (45 min)

**Context (5 min)**
1. How many new customers do you implement per year? Typical duration and effort (hours)? Team size and roles? Do you charge for implementation, and is it profitable?

**Current process (15 min)**
2. Walk me through your last implementation, from signed contract to go-live.
3. How do you handle customer data migration today? Which templates and tools? Who does it?
4. How is the product configured for a new customer (UI, scripts, import templates, APIs)?
5. How do you test and decide a customer is ready to go live?
6. Where do delays come from: customer data, mapping, configuration, testing, approvals, customer responsiveness?
7. How do you reuse what you learned from previous implementations?

**Pain and priority (10 min)**
8. Rate 1–5 the pain of: data migration, configuration, testing/UAT, requirements/fit-gap, customer follow-ups, go-live readiness.
9. What does a one-month delay in go-live cost you (revenue start, consultant cost, churn risk)?
10. Have you tried to automate any of this? Tools used (Rocketlane, GUIDEcx, Flatfile, in-house scripts)? What happened?

**Solution and commercials (10 min)**
11. (Show the prototype.) What would you need to trust this with your customers' data?
12. Would your customers use a portal to upload and fix their own data and approve steps?
13. How do you buy tools like this? Who decides, and who must approve (CTO, security)?
14. Pricing test: "If this cut migration and setup effort by ~30% and go-live by 2–4 weeks, would ~US$X per implementation plus a platform fee be reasonable?" (Use the Van Westendorp price questions.)

**Close (5 min)**
15. Would you pilot this with 2–3 upcoming customer implementations in the next 3 months, with sandbox/API access and baseline data?

## 3. CTO / technical questions

- What APIs exist (REST/OData/GraphQL)? Is there documentation or an OpenAPI spec?
- Which master entities and setup areas can be written via API? Is there a bulk import?
- How are sandboxes or test instances created per customer?
- Auth model (OAuth client credentials, API keys), permission scoping, rate limits
- Security requirements for a third party to access customer instances (questionnaire, DPA, pen-test report)

## 4. Scoring

Record per interview: product, company size, implementations/year, effort per implementation, pain scores, cost of delay, API readiness (0–3), price reaction, pilot willingness. **G0 signal:** ≥ 60% rate migration/setup pain ≥ 4/5; ≥ 3 companies with API readiness ≥ 2; 1–2 signed LOIs.

## 5. Design-partner LOI — key terms

- Pilot: adapter build for their product + 2–3 real customer implementations, within ~6 months
- The product company provides: API documentation, sandbox access, a technical contact (≥ 2 h/week), an implementation champion (≥ 4 h/week), baseline data (hours and timelines for comparable past implementations), and customer consent for data processing
- UNIFY provides: free adapter onboarding, free pilot, white-glove playbook setup, priority roadmap input
- Success criteria agreed upfront (below); if met, an annual contract at the design-partner price (50% off list in year 1)
- Mutual NDA + DPA; the product company owns its playbooks and knowledge pack content; UNIFY owns the platform and adapter framework; case study with approval

## 6. Technical proof-of-concept success criteria

| Criterion | Target |
|---|---|
| Adapter | Contract implemented; ≥ 3 entities + ≥ 1 setup area via API; conformance suite green |
| Mapping top-1 accuracy on real customer files | ≥ 75% (PoC), ≥ 85% (MVP) |
| Lineage | 100% records traceable or in exceptions |
| Duplicate creation on retry | 0 |
| Reconciliation | Counts match; variances explained |
| Effort | Migration + setup hours ≥ 30% below baseline |
| Cycle time | File receipt → approved sandbox migration ≥ 40% faster |
