# 03 — SaaS Business Model & Pricing

> All prices are hypotheses to test with design partners. Use [15-validation-kit.md](15-validation-kit.md) to test them.

## 1. Value math (why an ERP product company pays)

Typical mid-size ERP product company:

| Item | Assumption |
|---|---|
| New customer implementations per year | 100 |
| Implementation effort per customer | 150–600 consultant hours |
| Share in data migration + configuration + testing + readiness | 35–50% |
| Fully loaded consultant cost | US$25/hr (India) to US$90/hr (UK/EU) |
| UNIFY reduction on those workstreams (target, to prove) | 30–50% |
| Average subscription value of a new customer (ARR) | US$10k–50k |

**Value levers per year (100 implementations):**
1. **Delivery cost saved:** ~25–150 hours × 100 customers × US$25–90 ≈ **US$60k–1.3 m**
2. **Faster revenue:** going live 2–4 weeks earlier on 100 customers × US$20k ARR ≈ **US$400k–1.5 m** of revenue pulled forward
3. **Capacity:** onboard 30–50% more customers without hiring
4. **Retention:** fewer failed or delayed go-lives, so fewer year-1 churns

Pricing target: capture **10–20% of the measurable delivery-cost and time-to-revenue value**.

## 2. Pricing model

**Platform subscription + per-implementation fee**, with an **adapter onboarding fee** for each product connected. Per-implementation pricing aligns with the product company's own business: it can be bundled into the implementation fee they charge their customers.

| Component | Indicative price (global) | Notes |
|---|---|---|
| **Adapter onboarding** (one-time, per product) | US$15k–40k | Building and certifying the adapter + knowledge pack for their product with their team. Waived/discounted for design partners. |
| **Platform subscription** | Growth: US$2,500 / month · Scale: US$6,000 / month · Enterprise: custom | Includes internal users, customer portal, playbooks, governance, a number of included implementations |
| **Per implementation** | US$300–1,200 per customer implementation (by tier and volume) | Counted when a customer project is activated; includes migration records up to a limit |
| **Usage overage** | Metered | Records migrated, AI credits beyond fair use |
| **Enterprise / Private** | US$150k+ ACV | Large product companies; single-tenant/VPC, BYO model keys, SSO/SCIM, custom SLA |

**Plans (illustrative):**

| Plan | Implementations/yr included | Price |
|---|---|---|
| Growth | 40 | ~US$30k/yr + US$600 per extra implementation |
| Scale | 150 | ~US$72k/yr + US$400 per extra implementation |
| Enterprise | Custom | US$150k+ |

**India price book:** ~40–50% of global list, billed in INR with GST.

**Design-partner terms:** adapter onboarding free; free pilot on 2–3 customer implementations; 50% discount in year 1 in exchange for a case study, a reference and roadmap input.

## 3. Additional revenue (later)

| Stream | When | Notes |
|---|---|---|
| Embedded "Powered by UNIFY" migration inside the product company's own app (API/white-label) | Year 2 | The product company's customers self-migrate through UNIFY inside its product |
| Industry and localisation packs | Year 2–3 | e.g., GST e-invoicing setup, GCC VAT, industry test packs |
| Other B2B product categories (WMS, logistics, CRM…) | Year 2+ | Same pricing structure |
| Professional services (adapters, playbook design) | From day 1, capped at <20% of revenue | Avoid becoming a services company |

## 4. Unit economics targets (steady state)

| Metric | Target |
|---|---|
| Gross margin | ≥ 75% (AI + compute cost ≤ US$30–100 per implementation) |
| ACV (blended) | US$50k–100k |
| CAC payback | < 18 months |
| Net revenue retention | > 125% (implementation volume grows with the product company's sales) |
| Logo churn | < 8% / year |

## 5. Revenue scenario (illustrative only)

| | End of Year 1 | End of Year 2 | End of Year 3 |
|---|---|---|---|
| Paying ERP product companies | 3–5 | 15–25 | 50–80 (incl. other B2B categories) |
| Products connected | 3–5 | 15–25 | 50+ |
| ARR | US$0.15–0.35 m | US$1.2–2.5 m | US$5–8 m |

A small number of customers, but large contracts with strong expansion as each product company's implementation volume grows. The key constraint is **adapter onboarding speed**, which is why the SDK is strategic.

## 6. Billing and SaaS mechanics to build

- Plans, entitlements and limits enforced in the backend (implementations, records, AI credits)
- Usage metering (`usage_records`) per tenant, product and implementation project
- Stripe (global) + Razorpay (India); GST invoicing
- Demo tenant with a mock ERP product for sales
- Usage dashboard and alerts at 80% / 100% of limits
