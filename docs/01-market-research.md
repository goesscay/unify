# 01 — Market Research & Competitive Analysis

> Research snapshot: October–November 2026. Market-size figures come from analyst press releases and secondary sources; treat them as directional. Sources are listed at the end.

## 1. Market size

| Market | Estimate | Source / note |
|---|---|---|
| ERP software (global) | ~US$117–120 bn by 2030, ~10–12% CAGR | Allied Market Research; other estimates vary |
| ERP implementation services | ~US$29.5 bn (2025) → ~US$44 bn (2031), ~7.3% CAGR | Mordor Intelligence |
| ERP system integration & consulting | +US$14.7 bn growth 2024–2029, ~10.6% CAGR | Technavio |
| Services share of ERP | Services segment expected to grow fastest (~10.7% CAGR) | Allied Market Research |

**What this means for UNIFY.** Every ERP sale is followed by an implementation, and much of that work is done by the product company's own implementation team. Implementation cost and duration decide how fast the product company recognises revenue, how many customers its team can onboard, and whether customers churn in year 1. UNIFY sells to the product company to make that engine faster and cheaper.

### Bottom-up sizing (hypothesis, to validate)

| Input | Assumption |
|---|---|
| ERP product companies worldwide (cloud/SaaS and hybrid; horizontal, vertical and regional) | ~2,000–4,000 (software directories list 1,000+ ERP products; many more vertical and regional ERPs) |
| Of which mid-size with in-house implementation teams and APIs (the ICP) | ~600–1,200 |
| Target ACV | US$40k–150k |
| **Serviceable market (ERP only)** | **~US$50–150 m ARR** |
| Expansion: other configurable B2B products with implementation teams (WMS, TMS/logistics, CRM, manufacturing/MES, HCM/payroll, fintech, healthcare SaaS) | ×5–10 the number of companies; **US$0.5–1 bn+ ARR** opportunity |

ERP is the proving ground, as the SME blueprint says: it is the hardest case (master data, finance, configuration, integrations, reconciliation). If the engine works for ERP, it works for simpler products.

## 2. The problem, quantified

- **Implementations are slow and over budget.** Panorama Consulting surveys report most ERP projects take longer than planned (~61%) and roughly half go over budget (~53%).
- **The product company pays for it.** Implementation is often priced at or below cost to win the deal. Every extra week delays subscription revenue, ties up scarce consultants, and raises churn risk.
- **Data migration is the most underestimated workstream.** Every customer arrives with Excel files, legacy exports (Tally, QuickBooks, old ERPs) and dirty data. It is redone manually for every customer.
- **Repeatability is high, but reuse is low.** The same product, the same entities, the same setup sequence and the same test scripts appear in every implementation. Yet each one restarts from spreadsheets, and knowledge leaves with the consultant.
- **Implementation capacity caps growth.** Sales can close more customers than the implementation team can onboard. Hiring ERP consultants is slow and expensive.

## 3. Customer segments

| Segment | Examples | Pain | Fit for UNIFY | When |
|---|---|---|---|---|
| **A. Mid-size ERP product companies** (50–1,000 employees, in-house implementation team) | Regional and vertical ERPs (e.g., Indian cloud ERPs, GCC ERPs, industry ERPs for distribution, manufacturing, construction, retail) | Time-to-go-live, implementation margin, consultant capacity, early churn | **Beachhead.** Repeatable implementations, APIs available, fast decisions | Phase 0–GA |
| B. Large ERP product companies | Global mid-market/enterprise ERP publishers | Scale, partner quality | Long cycles; build their own tools | Later, enterprise edition |
| C. Other configurable B2B product companies | WMS, logistics SaaS, CRM, manufacturing/MES, HCM/payroll, fintech | Same implementation pain | Same engine, new adapters and packs | After ERP pilot |
| D. Small ERP startups (<50 employees) | Early-stage ERPs | Real pain, small budget | Self-serve "Starter" later | Later |

**Beachhead ICP detail:** see [02-product-strategy.md](02-product-strategy.md#3-ideal-customer-profile-beachhead).

## 4. Competitive landscape

### 4.1 Direct and near-direct competitors

| Player | What they do | Strength | Gap UNIFY can exploit |
|---|---|---|---|
| **Rocketlane** (Nitro AI) | Implementation/onboarding PSA platform for SaaS product companies' implementation teams; agentic AI features | Strong project management, customer portal, PSA; many SaaS customers; published time-to-value case studies | **Tracks** the implementation; does **not execute** data migration, configuration or tests inside the customer's product |
| **GUIDEcx** | Customer onboarding project portal for SaaS companies | Customer-facing UX; onboarding teams of 5–30 | Tracking only; no execution |
| OnRamp, Arrows, Valuecase, Tandem | Customer onboarding portals | Lightweight, cheaper | Tracking/collaboration only |
| **superglue** (YC, ~US$0.5 m pre-seed) | AI implementation automation (migration and config), currently sold through ERP marketplaces | Same execution thesis, shipping | Early; narrow product coverage; no governed project workspace or readiness |
| Tato (~US$5 m raised) | "Fixing ERP implementations with AI" | Funded, ERP-specific | To be analysed in Phase 0 |
| Flatfile, OneSchema, Dromo, Osmos | Embeddable customer data import/onboarding (CSV mapping, validation) for SaaS products | Very good data-import UX; product companies already embed them | Import step only: no implementation workflow, configuration, testing, reconciliation or readiness |
| **In-house tools** | Product company's own import templates, scripts, setup wizards | Fit their product exactly | Expensive to maintain; no AI; no governance; not reusable |

**Key insight:** UNIFY sits between the **onboarding project tools** (Rocketlane, GUIDEcx), which track but don't execute, and the **data-import widgets** (Flatfile, OneSchema), which do one step. UNIFY is the governed engine that **does the implementation work end to end** inside the product.

### 4.2 Adjacent categories

| Category | Players | Relationship |
|---|---|---|
| Enterprise data migration | Syniti, Informatica, Talend | Too heavy for mid-size product companies |
| iPaaS / integration | Celigo, Boomi, Workato, MuleSoft | Integrate with, don't replace |
| Test automation | Tricentis, Leapwork, Worksoft | Export to later |
| General AI | Consultants using ChatGPT/Claude ad hoc | The "do nothing" option; UNIFY wins on governance, execution, traceability and reuse |

### 4.3 Platform signal

SAP launched AI migration assistants on Cloud ALM (Sapphire 2026), and Oracle and Microsoft are embedding agents in their ERPs. This **validates the thesis**: the largest ERP publishers are investing in AI-automated implementation for themselves. Mid-size ERP product companies cannot afford to build this. **UNIFY gives every mid-size ERP product company the implementation automation that only SAP-scale companies can build in-house.** That is the core pitch.

## 5. White space: where UNIFY wins

1. **Execution, not tracking**: migration, configuration and tests run inside the customer's instance of the product through governed adapters.
2. **"Connect your product once"**: an adapter SDK and capability registry, so the product company gets automation without building it.
3. **Governed AI**: approval gates, record-level traceability, reconciliation and audit; safe to use on customers' financial data.
4. **Compounding knowledge**: every implementation improves that product's mappings, rules and playbooks.
5. **Mid-size and regional ERP product companies** (India, GCC, SEA, Europe), which are underserved by enterprise tools and can't build their own.

## 6. Key takeaways for planning

- The thesis is validated (SAP's assistants, superglue, Tato, the Rocketlane AI push). The window is open but narrowing.
- **The design-partner product company is everything**: its product, APIs and implementation team define the first adapter and prove the value.
- Position as **"the AI implementation engine for your product"**, not as onboarding project management.

## Sources

- [Mordor Intelligence — ERP Implementation Services Market](https://www.mordorintelligence.com/industry-reports/erp-implementation-services-market)
- [Technavio — ERP System Integration & Consulting Market](https://technavio.com/report/erp-system-integration-and-consulting-market-analysis)
- [Allied Market Research via PR Newswire — ERP market to US$117 bn by 2030](https://prnewswire.co.uk/news-releases/enterprise-resource-planning-erp-market-to-reach-117-09-billion-globally-by-2030-at-10-0-cagr-allied-market-research-302255102.html)
- [Computerworld — Panorama ERP survey](https://www.computerworld.com/?p=1409911)
- [CIO — ERP project woes survey](https://www2.cio.com.au/article/454319/erp_software_project_woes_continue_mount_survey_says/)
- [Rocketlane — GUIDEcx competitors](https://rocketlane.com/blogs/guidecx-competitors)
- [Rocketlane — SupportLogic case study](https://rocketlane.com/customers/how-supportlogic-accelerated-their-time-to-value-by-60-by-moving-from-guidecx-to-rocketlane)
- [Rocketlane — AI data migration](https://www.rocketlane.com/blogs/ai-data-migration)
- [Tandem — GUIDEcx vs Rocketlane](https://usetandem.ai/blog/guidecx-vs-rocketlane)
- [superglue on the Sage Intacct Marketplace — CPA Practice Advisor](https://www.cpapracticeadvisor.com/?p=189139)
- [superglue — PitchBook profile](https://pitchbook.com/profiles/company/761476-69)
- [Tato raises US$5 m — Pulse 2.0](https://pulse2.com/tato-5-million-raised-for-fixing-erp-implementations-with-ai-based-platform/?noamp=mobile)
- [SAP — Migration and Modernization Assistants FAQ](https://www.sap.com/documents/2026/06/3eabc539-5b7f-0010-bca6-c68f7e60039b.html)
- [SAPinsider — Sapphire 2026: AI agents, workflows, cloud migration](https://sapinsider.org/articles/sap-sapphire-2026-ai-agents-industry-workflows-cloud-migration/)
- [Energent — AI-powered ERP migration tools 2026](https://www.energent.ai/energent/compare/en/ai-powered-erp-migration)
