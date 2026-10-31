# 02 — Product Strategy & Positioning

## 1. Vision, mission, positioning

- **Vision:** every ERP implementation runs like a factory: repeatable, evidence-based and mostly automated, with experts spending their time on judgement rather than spreadsheets.
- **Mission:** give ERP implementation teams an AI engine that *executes* governed implementation work across any ERP, and gets smarter with every project.
- **Positioning statement:** *For ERP implementation partners and ERP vendors' delivery teams who lose margin to repetitive migration, configuration and testing work, UNIFY is an AI implementation engine that executes that work through governed connectors with full traceability. Unlike project-tracking tools (Rocketlane, GUIDEcx) or single-ERP native tools, UNIFY does the work, across ERPs, and turns each partner's know-how into reusable automation.*
- **Tagline (existing):** One engine. Any product. Automated implementation.

## 2. Key strategic decision: who is the customer?

The blueprint names **ERP SaaS vendors** as the primary buyer. The founder's goal is **all major ERP implementation companies**. These are different businesses:

| | ERP vendor-first (blueprint) | Implementation partner-first (recommended) |
|---|---|---|
| Who builds the connector | The vendor helps; one adapter serves one vendor | UNIFY builds it; one adapter serves *hundreds of partners* |
| Number of buyers per adapter | 1 | 100s–1000s |
| Sales cycle | 6–12 months, product and engineering approvals | 1–3 months, practice head or founder decides |
| Economic pain | Indirect (time-to-value, churn) | Direct (consultant hours = margin on fixed-price deals) |
| Risk | Vendor builds it in-house | ERP platform adds native AI tooling |

**Recommendation:** **partner-first, vendor-compatible.** The same multi-tenant product serves both. Tenant type = `implementation_partner` or `software_vendor`. A vendor tenant can also publish its product adapter and playbooks to its partner network. That is the long-term flywheel: vendors bring UNIFY into their partner ecosystem.

## 3. Ideal customer profile (beachhead)

- ERP implementation partner/VAR with **15–300 consultants**
- Implements **mid-market cloud ERP**: Dynamics 365 Business Central (first), then NetSuite, Odoo, SAP Business One, Acumatica, Sage Intacct
- **≥ 15 new implementations a year**, mostly fixed-price or capped
- Has a delivery methodology and templates (Excel mapping sheets, setup checklists) but no automation
- Geography: India, Middle East, UK and the US (English-speaking, strong BC/Odoo/SAP B1 channels)
- Buyer: founder/CEO, Head of Delivery, Practice Lead. Champion: senior functional or data-migration consultant.

## 4. The wedge: what we sell first

The blueprint's MVP covers all eight modules in six months with six people. That is too broad to make any one module excellent. The recommended wedge:

### "UNIFY Migration & Configuration Workbench" for one ERP

1. **Data migration factory**: upload legacy extracts (Excel/CSV, later Tally/QuickBooks/legacy DB), profile, AI-map, apply deterministic validation, cleanse, approve, load to the ERP sandbox, reconcile. Master entities: customers, vendors, items, chart of accounts, opening balances, and later open AR/AP and inventory on hand.
2. **Configuration from templates**: apply the partner's standard setup (posting groups, payment terms, tax setup, number series, dimensions) via API, read back and verify, and produce a "configuration workbook" as evidence.
3. **Governance layer**: projects, approvals, audit, exceptions, evidence. This is enough project structure to run the workbench, not a full PSA.
4. **Customer portal**: the customer uploads data, fixes exceptions and approves mappings and reconciliations. This removes the email-and-Excel ping-pong, which is a major hidden time sink.

Why this wedge:
- Hours saved are **measurable** (baseline vs. pilot), which makes the ROI story easy to sell.
- It is a pain partners feel on **every** project.
- It needs the **hardest core capabilities** (connector, governed execution, traceability), so everything later reuses it.
- Project tracking (Rocketlane) and AI fit-gap writing (ChatGPT) are crowded or commoditising; governed execution is not.

### Expansion path

Wedge → **Test & Readiness** (requirement-linked test generation and execution, go-live readiness pack) → **Discovery & Fit-Gap** (requirements extraction, fit-gap against the ERP knowledge pack, plan generation) → **Second and third ERP adapters** → **Hypercare** → **Vendor network / marketplace**.

## 5. Product principles

1. **AI proposes, deterministic services execute, humans approve material actions.** (Kept from the blueprint; this is the trust model.)
2. **Never invent.** Agents can only use capabilities in the registry. Every mapping has a rationale, a confidence score and a source.
3. **Evidence by default.** Every action leaves before/after evidence, which feeds the readiness pack automatically.
4. **Learn from approved outcomes only.** Approved mappings, rules and configurations become reusable knowledge, scoped to the tenant unless explicitly shared.
5. **Work where consultants work.** Excel import/export at every step, so the product never forces "all-or-nothing" adoption.
6. **Sandbox first; production only by explicit authorisation.**

## 6. Moat and defensibility

| Moat | How it builds |
|---|---|
| **Implementation knowledge graph** | Every approved mapping, transformation, validation rule, configuration and test becomes a reusable, versioned asset. Mapping accuracy improves per ERP and per source system (e.g., "Tally → BC customer master"). |
| **Partner IP lock-in (positive)** | A partner's methodology lives in UNIFY as playbooks and packs. Switching means losing their automated factory. |
| **Connector depth** | Deep, tested adapters for each ERP (including edge cases, dependencies and setup order) are expensive to rebuild. |
| **Evidence and trust** | Audit-grade traceability that auditors and customers accept, plus certifications (SOC 2, ISO 27001). |
| **Network (later)** | ERP vendors publish adapters and packs to their partners; partners share or sell packs in a marketplace. |

## 7. What UNIFY is not

- Not an ERP, not a generic chatbot, not a PSA or time-tracking tool (integrate with Rocketlane, Jira, MS Project or PSA tools instead), and not an iPaaS for ongoing production integrations (integrate with Celigo, Boomi or Power Automate).
- Not autonomous in production. Production cutover is always human-authorised.

## 8. Strategic risks to the positioning

| Risk | Response |
|---|---|
| ERP vendor ships native AI implementation tooling (SAP already has) | Start with mid-market ERPs; be cross-ERP; partner with vendors (ISV/marketplace listings) |
| superglue or another startup moves faster | Narrow and deep beats broad: one ERP done excellently, plus governance and customer portal |
| Partners see UNIFY as reducing billable hours | Sell **margin on fixed-price work and capacity** ("deliver 30% more projects with the same team"), not "fewer hours" |
| Data sensitivity blocks adoption | Sandbox-first, regional hosting, SOC 2 early, private deployment option for large SIs |

## 9. Naming note

"Unify" is a crowded mark (e.g., *Unify* is a well-known communications brand owned by Atos/Mitel, and *Unify Square* exists). Run a trademark search in the target classes (9 and 42) for India, the US, the UK and the EU before investing in the brand. Consider a distinct compound name (e.g., "UNIFY Implement", "Unifyr") if conflicts appear.
