# 02 — Product Strategy & Positioning

## 1. Vision, mission, positioning

- **Vision (from the SME blueprint):** any company offering a configurable B2B software product should be able to connect that product to UNIFY, onboard a customer and automate a substantial share of repeatable implementation work, without rebuilding the implementation engine for every product.
- **Mission:** give ERP product companies an AI engine that *executes* governed implementation work inside their product and gets smarter with every customer they onboard.
- **Positioning statement:** *For ERP product companies whose growth and margins are limited by slow, manual customer implementations, UNIFY is an AI implementation engine that connects to their product and executes data migration, configuration, testing and readiness work with full traceability. Unlike onboarding project tools (Rocketlane, GUIDEcx) that only track work, or data-import widgets (Flatfile) that handle one step, UNIFY does the implementation end to end, and the product company never has to build it.*
- **Tagline:** One engine. Any product. Automated implementation.

## 2. Who the customer is

**ERP product companies**: they own the ERP, sell it, and implement it for their customers with an in-house implementation team (sometimes helped by a few partners).

| Who in the product company | What they care about |
|---|---|
| CEO / founder | Faster revenue recognition, growth without linear hiring, lower churn |
| VP / Head of Implementation (Customer Delivery, Professional Services) | Time-to-go-live, consultant capacity, consistency, margin |
| CTO / Head of Product | Safe API access, no load on engineering, security |
| Implementation consultants, data migration specialists, QA | Less spreadsheet work, fewer escalations |
| The product company's customers (end customers) | Visibility, fewer requests, a smooth go-live |

**Later:** other configurable B2B product companies (WMS, logistics, CRM, manufacturing, fintech), using the same engine.

## 3. Ideal customer profile (beachhead)

- Owns a **cloud (or hosted) ERP product** with **documented REST/OData APIs** (or willing to expose some) and **sandbox/test instances** per customer
- **50–1,000 employees**; an implementation team of **10–150 people**
- **30–500 new customer implementations a year**; typical implementation **4–24 weeks**
- Targets SMB/mid-market customers in distribution, manufacturing, retail, services and similar industries
- Has a standard implementation methodology and templates (migration templates, setup checklists) but little automation
- Geography to start: **India**, then GCC, SEA, UK/EU (many regional and vertical ERP companies; GST/VAT complexity)
- **Buyer:** CEO/founder or VP Implementation. **Champion:** implementation lead or data migration lead. **Approver:** CTO (API access, security).

## 4. The wedge: what we sell first

The SME blueprint's MVP covers all eight modules in six months. That is too broad for one team to make any module excellent. The recommended order:

### Wedge: "Connect your product → automate customer data migration and setup"

1. **Product connection (adapter)**: UNIFY connects to the product company's ERP through the adapter contract and capability registry (blueprint section 10). For the first design partner, UNIFY engineers build the adapter with the product company's team. Later, the SDK makes this self-serve.
2. **Customer data migration factory**: the end customer uploads legacy data (Excel/CSV, Tally, QuickBooks, old ERP extracts). UNIFY profiles, AI-maps to the product's entities, validates deterministically, routes exceptions to the customer to fix, gets approval, loads to the customer's sandbox, and reconciles. Entities: customers, suppliers, items, chart of accounts, branches/locations, opening balances (blueprint flagship workflow).
3. **Standard configuration**: apply the product company's configuration templates with AI-proposed values from the customer's questionnaire, then execute, read back and verify.
4. **Governed implementation workspace**: projects from playbooks, approvals, exceptions, audit, evidence. This is the product company's **internal Delivery Control Tower** (mock-up 1).
5. **Customer portal**: the end customer uploads data, fixes exceptions, approves and sees milestones and readiness (mock-up 2, the Customer Delivery Workspace).

Why this wedge:
- It attacks the **biggest repeatable time sink** in every implementation, and the savings are measurable.
- It proves the **hardest capability**: a product-independent engine executing safely inside a real product.
- The two mock-ups become real screens immediately, which strengthens demos and sales.

### Expansion path

Wedge → **Testing & go-live readiness** (requirement-linked tests, readiness gates, cutover) → **Discovery & planning** (requirements extraction, fit-gap against the product's capabilities, plan generation) → **Integrations** (REST/webhook/SFTP setup and testing) → **Hypercare** → **Self-serve adapter SDK** → **Other B2B product categories** (WMS, logistics, CRM…).

## 5. Product principles (kept from the blueprint)

1. **AI proposes, deterministic services execute, humans approve material actions.**
2. **Never invent.** Agents can only use capabilities in the product's capability registry.
3. **Evidence by default.** Every action leaves before/after evidence, feeding readiness automatically.
4. **Learn from approved outcomes only**, scoped to the product company (tenant).
5. **Work where consultants work.** Excel import/export at every step.
6. **Sandbox first; production only by explicit authorisation.**
7. **Organise the UX around the implementation lifecycle and exceptions**, not agent names (blueprint Part J).

## 6. Moat and defensibility

| Moat | How it builds |
|---|---|
| **Adapter SDK and capability registry** | Each new product connects faster. Accumulated patterns (auth, metadata discovery, bulk load, read-back) cut a new adapter from months to weeks. |
| **Product implementation knowledge** | Every approved mapping, rule, configuration and test for a product becomes reusable for that product company's next customer. Mapping accuracy compounds per product and per source system (e.g., "Tally → this ERP's customer master"). |
| **Embedded in the product company's delivery process** | Playbooks, templates and the customer portal become how they implement. Switching means rebuilding their implementation engine. |
| **Cross-product learning (anonymised, opt-in)** | Common source systems (Tally, QuickBooks, Excel formats) and common entities improve for all tenants. |
| **Trust** | Audit-grade traceability, SOC 2 / ISO 27001, data residency. |

## 7. What UNIFY is not (from the blueprint)

Not a replacement ERP, not a generic chatbot, not merely a project tracker, not a full PSA or ticketing product, and not an iPaaS for ongoing production integrations. Production go-live is never autonomous.

## 8. Strategic risks

| Risk | Response |
|---|---|
| The product company builds it in-house | Speed and breadth: UNIFY ships migration + config + governance + portal + AI evals in weeks; building that internally costs years of engineering |
| Product APIs too limited for setup/config | Phase 0 API spike with the design partner; co-develop missing APIs; bulk import as fallback |
| Each adapter becomes custom services work | Strict adapter contract; SDK; cap services; price adapter onboarding separately |
| Onboarding tools (Rocketlane) add execution features | Go deep on ERP execution (finance-grade migration and reconciliation) where generic tools can't follow |
| Data sensitivity | Sandbox-first, regional hosting, SOC 2 early, private deployment option |

## 9. Naming note

"Unify" is a crowded mark (e.g., a well-known communications brand owned by Atos/Mitel, and Unify Square). Run a trademark search in classes 9 and 42 for India, the US, the UK and the EU before investing in the brand.
