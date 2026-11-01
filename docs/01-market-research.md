# 01 — Market Research & Competitive Analysis

> Research snapshot: October 2026. Market-size figures come from analyst press releases and secondary sources; treat them as directional. Sources are listed at the end.

## 1. Market size

| Market | Estimate | Source / note |
|---|---|---|
| ERP software (global) | ~US$117–120 bn by 2030, ~10–12% CAGR | Allied Market Research; other analyst estimates vary |
| ERP implementation services | ~US$29.5 bn (2025) → ~US$44 bn (2031), ~7.3% CAGR | Mordor Intelligence |
| ERP system integration & consulting | +US$14.7 bn growth 2024–2029, ~10.6% CAGR | Technavio |
| Services share of ERP | Services segment expected to grow fastest (~10.7% CAGR) | Allied Market Research |

**What this means for UNIFY.** For every dollar of ERP licence, customers typically spend one to three dollars on implementation services. That service spend is labour, and labour is what UNIFY automates. Even a small share of the services pool is a large software market. For example, if tooling captures 3–5% of a ~US$30 bn services market, that is a US$0.9–1.5 bn tooling opportunity.

### Bottom-up sizing (hypothesis, to validate)

| Input | Assumption |
|---|---|
| Mid-market ERP implementation partners worldwide (SAP B1/S4 Public, Dynamics 365 BC/F&O, NetSuite, Odoo, Sage, Acumatica, Infor, Epicor, Zoho/Tally ecosystems) | ~20,000–30,000 firms (SAP alone claims 25,000+ partners; Microsoft's ecosystem is larger) |
| Firms with 10–500 consultants (realistic SaaS buyers) | ~6,000–10,000 |
| Average projects per firm per year | 10–60 |
| Target ACV | US$15k–60k |
| **Serviceable market (SAM)** | **~US$150–400 m ARR** for the mid-market partner segment alone, before large SIs |

## 2. The problem, quantified

- **Overruns are the norm.** Panorama Consulting surveys report that most ERP projects take longer than planned (~61%) and roughly half go over budget (~53%). Average projects ran ~US$7 m over ~18 months in the surveyed population (skewed to larger firms).
- **Data migration is the most underestimated workstream.** Estimates range from 10–15% of cost to much higher shares of the problems found. Every implementation repeats it, and it is still done in Excel, with VLOOKUPs and with consultants on the phone with customers.
- **Repeatability is high, but reuse is low.** Partners implement the same ERP dozens of times a year with the same master entities, the same setup sequence and the same test scripts. Yet each project restarts from spreadsheets. Knowledge leaves with the consultant.
- **Partner margins are pressured.** Customers want fixed-price, faster implementations. Vendors (SAP, Microsoft, Oracle) push "fast-track" methods. Partners that cannot industrialise delivery lose margin.

## 3. Buyer segments

| Segment | Examples | Pain | Willingness to buy tooling | Fit for UNIFY |
|---|---|---|---|---|
| **A. Mid-market ERP implementation partners / VARs** (10–500 consultants) | Dynamics 365 BC partners, NetSuite Solution Providers, SAP B1 partners, Odoo partners | Margin on fixed-price projects; consultant scarcity; repeat work | High: they buy tools to protect margin, and decisions are fast (founder/practice head) | **Best beachhead** |
| B. ERP software vendors with in-house implementation teams | Mid-size vertical ERPs, regional ERPs (e.g., Indian ERPs) | Time-to-value, onboarding cost, churn | Long sales cycle; one adapter serves one vendor | **Not a target customer** (founder decision). Engage only as ecosystem/marketplace partners. |
| C. Large global SIs | Accenture, Deloitte, Infosys, TCS, Wipro, Capgemini | Scale, consistency | Build their own tooling, long procurement, demand on-prem | Later: enterprise edition / OEM |
| D. ERP vendors' own programs | SAP Cloud ALM migration assistants, Microsoft FastTrack, NetSuite SuiteSuccess | Want partners to deliver faster | They are platforms and potential competitors | Partner with them, don't fight them |
| E. End customers doing self-implementation | SMBs implementing Odoo/BC themselves | No expertise | Low budget | Not a target; served indirectly |

**Conclusion:** the original blueprint named segment B (ERP vendors) by mistake. **Founder decision (1 Nov 2026): UNIFY serves implementation partners only**: segment A first, then segment C (large SIs) through an enterprise edition. ERP vendors (B, D) are platforms, marketplaces and ecosystem partners, not buyers. See [02-product-strategy.md](02-product-strategy.md).

## 4. Competitive landscape

### 4.1 Direct and near-direct competitors

| Player | What they do | Strength | Gap UNIFY can exploit |
|---|---|---|---|
| **superglue** (YC, ~US$0.5 m pre-seed, 2024) | "AI implementation platform for ERP partners/VARs"; agentic migration and config; Sage Intacct Marketplace connector (Aug 2026); claims a Sage VAR cut migration hours by 80% | Same thesis, already shipping; self-hosted option | Early; Sage-centric; migration-only; no project governance, customer portal or testing/readiness. **The closest threat. Watch closely.** |
| **Tato** (raised ~US$5 m) | "Fixing ERP implementations with an AI-based platform" | Funded, ERP-specific | Needs a deeper look; likely project and requirements focused |
| **Rocketlane** (Nitro AI) | Implementation/onboarding PSA platform with agentic AI | Strong project management, customer portal and PSA | Generic across SaaS; **does not execute** ERP migration or config |
| **GUIDEcx**, Arrows, OnRamp, Valuecase | Customer onboarding project portals | Customer-facing UX | Tracking only, no execution |
| NextDay Migration, Nominal | AI-assisted migrations to NetSuite/ERP | Fast migration claims | Single-ERP, migration-only, service-heavy |

### 4.2 Adjacent categories (pieces of the job)

| Category | Players | Relationship to UNIFY |
|---|---|---|
| Enterprise data migration | Syniti, SAP Data Services/Migration Cockpit, Informatica, Talend, Cloudsufi, DataLark | Enterprise-grade, expensive, SAP-heavy; overkill for mid-market. UNIFY is lighter, AI-first and cross-ERP |
| iPaaS / integration | Celigo, Boomi, Workato, MuleSoft, Make | UNIFY should *integrate with* these, not replace them |
| ERP test automation | Tricentis, Panaya, Worksoft, Leapwork | UNIFY generates requirement-linked tests; it can export to them later |
| Process mining / fit-gap | SAP Signavio, Celonis | Enterprise discovery; UNIFY's discovery is lightweight and document-based |
| ERP-native tooling | SAP Cloud ALM + Joule for Consultants, BC RapidStart/Configuration Packages, NetSuite SuiteSuccess/CSV import, Odoo import | Free with the ERP, single-ERP, not AI-orchestrated. UNIFY must *use* these as execution channels |
| General AI | ChatGPT/Claude/Copilot used ad hoc by consultants | The "do nothing" competitor: consultants paste spreadsheets into chatbots. UNIFY wins on governance, traceability, execution and reuse |

### 4.3 The platform threat: ERP vendors moving in

- **SAP (Sapphire 2026)** launched AI migration and modernization assistants built on **SAP Cloud ALM**, requiring Joule for Consultants/Developers. Secondary sources cite ~35% migration effort reduction (unverified). SAP and Palantir partnered on AI-supported data migration, with Accenture as the lead services partner.
- **Oracle** is embedding agents in Fusion and NetSuite (mostly for operations, less for implementation).
- **Microsoft** is pushing Copilot into Dynamics 365, including some setup and data assistance.

**Implication:** do not start with SAP S/4HANA. SAP will own AI tooling for its large-enterprise migrations. Mid-market ERPs (Business Central, NetSuite, Odoo, Acumatica, Sage, SAP B1) have weaker native implementation tooling and many more partners. **Cross-ERP neutrality** is something no ERP vendor can offer, and partners who implement two or three ERPs value it.

## 5. White space: where UNIFY wins

1. **Execution, not tracking.** Rocketlane and GUIDEcx track tasks; UNIFY performs the migration, configuration and tests through governed connectors.
2. **Governed AI.** Approval gates, record-level traceability, reconciliation and audit, which is what partners need to put AI near customer financial data.
3. **Cross-ERP, partner-owned knowledge.** A partner's playbooks, mapping templates, validation rules and test packs become reusable, versioned IP inside UNIFY, across every ERP they implement.
4. **The mid-market, which the ERP vendors and big data-migration tools underserve.**
5. **India + emerging markets.** Large partner ecosystems (SAP B1, Dynamics, Odoo, Tally/Zoho), price-sensitive, GST/e-invoicing complexity. This is a natural home market for a lean, India-built team, with global pricing.

## 6. Key market takeaways for planning

- The thesis is validated by others moving (superglue, Tato, SAP's assistants). The window is open but narrowing over roughly 12–24 months.
- Speed to a **working, real-ERP migration demo** matters more than breadth.
- Pick a **mid-market ERP with good APIs and a large partner channel** for adapter #1.
- Position as **"the implementation factory for ERP partners"**, not as "AI project management."

## Sources

- [Mordor Intelligence — ERP Implementation Services Market](https://www.mordorintelligence.com/industry-reports/erp-implementation-services-market)
- [Technavio — ERP System Integration & Consulting Market](https://technavio.com/report/erp-system-integration-and-consulting-market-analysis)
- [Allied Market Research via PR Newswire — ERP market to US$117 bn by 2030](https://prnewswire.co.uk/news-releases/enterprise-resource-planning-erp-market-to-reach-117-09-billion-globally-by-2030-at-10-0-cagr-allied-market-research-302255102.html)
- [Computerworld — Panorama ERP survey](https://www.computerworld.com/?p=1409911)
- [CIO — ERP project woes survey](https://www2.cio.com.au/article/454319/erp_software_project_woes_continue_mount_survey_says/)
- [erp-software.org — ERP migration](https://erp-software.org/en/glossary/erp-migration/)
- [superglue on the Sage Intacct Marketplace — CPA Practice Advisor](https://www.cpapracticeadvisor.com/?p=189139)
- [superglue — PitchBook profile](https://pitchbook.com/profiles/company/761476-69)
- [superglue — AI data migration tools comparison](https://superglue.ai/compare/ai-data-migration-tools/)
- [Tato raises US$5 m — Pulse 2.0](https://pulse2.com/tato-5-million-raised-for-fixing-erp-implementations-with-ai-based-platform/?noamp=mobile)
- [Rocketlane — AI data migration](https://www.rocketlane.com/blogs/ai-data-migration)
- [Rocketlane — GUIDEcx competitors](https://rocketlane.com/blogs/guidecx-competitors)
- [Tandem — GUIDEcx vs Rocketlane](https://usetandem.ai/blog/guidecx-vs-rocketlane)
- [SAP — Migration and Modernization Assistants FAQ](https://www.sap.com/documents/2026/06/3eabc539-5b7f-0010-bca6-c68f7e60039b.html)
- [SAPinsider — Sapphire 2026: AI agents, workflows, cloud migration](https://sapinsider.org/articles/sap-sapphire-2026-ai-agents-industry-workflows-cloud-migration/)
- [SAPinsider — SAP partner ecosystem with Anthropic, NVIDIA, Palantir](https://sapinsider.org/blogs/sap-expands-partner-ecosystem-for-autonomous-enterprise-with-anthropic-nvidia-and-palantir/)
- [SAP Learning — SAP Partner Network](https://learning.sap.com/courses/introducing-sap-s-core-business-processes-sap-for-industries-and-the-sap-partner-network/introducing-the-sap-partner-network-1)
- [Energent — AI-powered ERP migration tools 2026](https://www.energent.ai/energent/compare/en/ai-powered-erp-migration)
- [Ispirer — AI data migration in 2026](https://www.ispirer.com/blog/ai-data-migration)
