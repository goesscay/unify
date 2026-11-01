# UNIFY — Planning Documentation

**UNIFY — Universal AI Implementation Engine.** A multi-tenant SaaS platform that **ERP product companies** connect their product to. They then use it to onboard and implement each new customer: discovery, data migration, configuration, testing and go-live readiness, automated where repeatable, under human approval and with full audit.

> Status: planning baseline v0.3 · 1 Nov 2026 · prepared by the UNIFY product owners from the four source documents in the repository root plus market research. Every number, price and date is a hypothesis until the validation phase confirms it.

## Who UNIFY serves

**ERP product companies**: companies that own an ERP (or other configurable B2B) product, sell it to business customers, and run their own implementation teams to get each customer live. They struggle with slow, manual, inconsistent implementations. The source PDFs call them "ERP vendors"; this document set uses **ERP product company** (or "product company").

- **Now:** ERP product companies.
- **Later:** other configurable B2B product companies (WMS, logistics, CRM, manufacturing, fintech SaaS), using the same engine with new adapters and knowledge packs.

## Source documents (repository root)

| File | What it contains |
|---|---|
| `UNIFY Complete Product Vision to Validation Assumptions v2.pdf` | 24-page blueprint: vision, PRD, architecture, DB, agents, APIs, security, backlog, roadmap, assumptions |
| `UNIFY – Stakeholder Project Summary and Plan.pdf` | 9-page stakeholder summary and decisions requested |
| `UNIFY Internal Delivery Dashboard.html` | Mock-up: the product company's internal "Delivery Control Tower" |
| `UNIFY Customer Delivery Dashboard.html` | Mock-up: the end-customer delivery workspace |

The UNIFY blueprint is the foundation. These planning documents keep its direction and add market research, sharper scope, commercial model and execution detail.

## Planning document set

| # | Document | Purpose |
|---|---|---|
| 01 | [Market research & competitive analysis](01-market-research.md) | Market size, segments, competitors, white space |
| 02 | [Product strategy & positioning](02-product-strategy.md) | ICP, wedge, moat, product principles |
| 03 | [SaaS business model & pricing](03-business-model-pricing.md) | Packaging, pricing hypotheses, unit economics, value math |
| 04 | [Product requirements (PRD)](04-prd.md) | Personas, modules, functional and non-functional requirements |
| 05 | [Solution architecture](05-architecture.md) | System design, tech stack, tenancy, deployment, ADRs |
| 06 | [Data model](06-data-model.md) | Entities, tables, tenancy rules, migration data layers |
| 07 | [Product connector framework](07-connector-framework.md) | Adapter contract, capability registry, onboarding a product |
| 08 | [AI agents & governance](08-ai-agents-governance.md) | Agents, tool policy, approvals, evaluation, guardrails |
| 09 | [Security, compliance & trust](09-security-compliance.md) | Controls, certifications, data residency, threat model |
| 10 | [Roadmap & release plan](10-roadmap.md) | Phase 0 validation → MVP → GA → platform, with gates |
| 11 | [Product backlog](11-backlog.md) | Epics, user stories, acceptance criteria, priorities |
| 12 | [Go-to-market plan](12-go-to-market.md) | ICP, channels, design partners, sales motion |
| 13 | [Team, budget & operating plan](13-team-budget.md) | Hiring, budget, tools, rituals |
| 14 | [Risks, assumptions & open questions](14-risks-assumptions-questions.md) | Risk register, assumptions to validate, open questions |
| 15 | [Validation kit](15-validation-kit.md) | Interview script, design-partner LOI terms, PoC success criteria |

## Decision log

| Date | Decision |
|---|---|
| 1 Nov 2026 | **Customers are ERP product companies** that implement their own product for their customers (the UNIFY blueprint's direction). Other configurable B2B product companies follow after the ERP pilot. |

## The plan in one paragraph

Sell to **mid-size ERP product companies** (typically 50–1,000 employees, 30–500 new customer implementations a year, an in-house implementation team, and an API-accessible cloud product). These companies lose revenue, cash and customer satisfaction because every implementation is slow and manual. Win with a narrow, measurable wedge: **connect the product company's ERP through an adapter, then automate customer data migration and standard configuration**, wrapped in a governed implementation workspace and a customer portal (the two dashboard mock-ups). Prove faster time-to-go-live and lower implementation cost with **one design-partner product company on 2–3 real customer implementations**. Then expand to testing, readiness and discovery, more product companies, and other B2B product categories. The moat is the **adapter SDK** (connecting a new product in weeks, not months) plus the **implementation knowledge** (playbooks, mappings, rules, tests) that compounds with every customer the product company onboards.
