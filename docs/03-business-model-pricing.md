# 03 — SaaS Business Model & Pricing

> All prices are hypotheses to test in design-partner conversations. Use [15-validation-kit.md](15-validation-kit.md) to test them.

## 1. Value math (why a partner pays)

Typical mid-market cloud ERP implementation (BC/NetSuite/Odoo class):

| Item | Assumption |
|---|---|
| Total implementation effort | 400–2,000 consultant hours |
| Share in migration + configuration + testing + readiness | 30–45% |
| Blended consultant cost | US$40/hr (India offshore) to US$120/hr (US/UK) |
| UNIFY reduction on those workstreams (target, to prove) | 30–50% |
| **Hours saved per project** | ~40–400 hours |
| **Value per project** | ~US$2k (small India project) to ~US$40k (large US project) |

Pricing target: capture **15–25% of the value created**, i.e. roughly **US$1k–6k per project**. Secondary value: fewer go-live slips, less rework, faster cash collection on milestones, and junior consultants who can deliver senior-quality migrations.

## 2. Pricing model

A **platform subscription plus an active-project allowance**, with usage-based AI and records as a guard-rail. Avoid per-seat pricing. Partners want to invite customers, juniors and reviewers freely, and seat pricing discourages the customer-portal adoption that drives value.

| Plan | Who | Indicative price (global) | Includes |
|---|---|---|---|
| **Starter** | Small partners, 5–20 consultants | US$990 / month (annual) | 1 ERP adapter, up to 5 active projects, 500k migrated records/yr, standard packs, email support |
| **Growth** | 20–150 consultants | US$2,900 / month (annual) | 2 ERP adapters, up to 20 active projects, 3 M records/yr, custom packs, customer portal branding, SSO |
| **Scale** | 150+ consultants, multi-region | US$7,500+ / month | Unlimited adapters, 60+ projects, data residency choice, audit exports, premium SLA |
| **Enterprise / Private** | Large SIs, ERP vendors | Custom (US$100k+ ACV) | Single-tenant or VPC deployment, BYO model keys, custom adapters, vendor network features |
| **Additional active project** | Any | US$150–400 / project-month | Overage beyond the allowance |
| **AI and records overage** | Any | Metered | Fair-use protection for LLM cost |

**India price book:** roughly 40–50% of global list (e.g., Starter ₹39,000/month, Growth ₹1.1 lakh/month), billed in INR with GST.

**Design-partner terms:** free 90-day pilot → 50% discount for year 1 in exchange for a case study, a reference and roadmap input.

## 3. Alternative and complementary revenue

| Stream | When | Notes |
|---|---|---|
| Per-project pricing (pay-as-you-go) | MVP / early | Easy for partners to pass through to the customer as a "migration accelerator" line item. Good for land. |
| ERP vendor licence (vendor tenant + partner network) | Year 2 | The vendor pays to provide UNIFY to its partners. A strong distribution lever. |
| Knowledge-pack marketplace (rev-share) | Year 3 | Partners and vendors sell industry/localisation packs (e.g., "GCC VAT for BC") |
| Professional services (adapter builds, onboarding) | From day 1, capped at <20% of revenue | Fund custom adapters, but avoid becoming a services company |

## 4. Unit economics targets (steady state)

| Metric | Target |
|---|---|
| Gross margin | ≥ 75% (watch LLM and compute cost per project; budget ≤ US$50–150 of AI cost per project) |
| ACV (blended) | US$25k–40k |
| CAC payback | < 18 months |
| Net revenue retention | > 120% (expansion via more projects and more ERPs) |
| Logo churn | < 10% / year |
| LLM cost guard | Per-tenant budgets, caching, smaller models for classification, large models only for mapping/reasoning |

## 5. Revenue scenario (illustrative only)

| | End of Year 1 | End of Year 2 | End of Year 3 |
|---|---|---|---|
| Paying partners | 8–15 | 50–80 | 180–250 |
| ERP adapters | 1–2 | 3–4 | 6+ |
| ARR | US$0.15–0.35 m | US$1.5–2.5 m | US$6–9 m |

This assumes a working wedge by month 6–7 and a design-partner-led launch. Fundraising should be timed around evidence from the pilots (see [10-roadmap.md](10-roadmap.md)).

## 6. Billing and SaaS mechanics to build

- Plans, entitlements and limits enforced in the backend (active projects, adapters, records, AI credits)
- Usage metering (`usage_records`) per tenant, project and agent run
- Stripe (global) plus Razorpay (India) for subscriptions and invoices; GST invoicing
- Trials and sandboxed demo tenant with a mock ERP
- Customer-facing usage dashboard and alerts at 80% and 100% of limits
