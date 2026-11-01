# UNIFY — Planning Documentation

**UNIFY — Universal AI Implementation Engine.** A multi-tenant SaaS platform that ERP implementation companies use to run and automate the repeatable parts of every customer implementation (discovery, data migration, configuration, testing and go-live readiness) under human approval and full audit.

> Status: planning baseline v0.1 · 31 Oct 2026 · prepared from the four source documents in the repository root plus market research. Every number, price and date is a hypothesis until the validation phase confirms it.

## Source documents (repository root)

| File | What it contains |
|---|---|
| `UNIFY Complete Product Vision to Validation Assumptions v2.pdf` | 24-page blueprint: vision, PRD, architecture, DB, agents, APIs, security, backlog, roadmap, assumptions |
| `UNIFY – Stakeholder Project Summary and Plan.pdf` | 9-page stakeholder summary and decisions requested |
| `UNIFY Internal Delivery Dashboard.html` | Mock-up: internal portfolio "Delivery Control Tower" |
| `UNIFY Customer Delivery Dashboard.html` | Mock-up: customer-facing delivery workspace |

## Planning document set

| # | Document | Purpose |
|---|---|---|
| 01 | [Market research & competitive analysis](01-market-research.md) | Market size, buyer segments, competitors, threats, white space |
| 02 | [Product strategy & positioning](02-product-strategy.md) | Who we serve first, why, the wedge, moat, key strategic decisions |
| 03 | [SaaS business model & pricing](03-business-model-pricing.md) | Packaging, pricing hypotheses, unit economics, value math |
| 04 | [Product requirements (PRD)](04-prd.md) | Personas, modules, functional and non-functional requirements |
| 05 | [Solution architecture](05-architecture.md) | System design, tech stack, tenancy, deployment, ADRs |
| 06 | [Data model](06-data-model.md) | Entities, tables, tenancy rules, migration data layers |
| 07 | [ERP connector framework](07-connector-framework.md) | Adapter contract, capability registry, first ERP choice |
| 08 | [AI agents & governance](08-ai-agents-governance.md) | Agents, tool policy, approvals, evaluation, guardrails |
| 09 | [Security, compliance & trust](09-security-compliance.md) | Controls, certifications, data residency, threat model |
| 10 | [Roadmap & release plan](10-roadmap.md) | Phase 0 validation → MVP → GA → platform, with gates |
| 11 | [Product backlog](11-backlog.md) | Epics, user stories, acceptance criteria, priorities |
| 12 | [Go-to-market plan](12-go-to-market.md) | ICP, channels, design partners, sales motion |
| 13 | [Team, budget & operating plan](13-team-budget.md) | Hiring, budget, tools, rituals |
| 14 | [Risks, assumptions & open questions](14-risks-assumptions-questions.md) | Risk register, assumptions to validate, questions for the founder |
| 15 | [Validation kit](15-validation-kit.md) | Interview script, design-partner LOI terms, PoC success criteria |

## Decision log

| Date | Decision |
|---|---|
| 1 Nov 2026 | **Customers are ERP implementation partners only.** The blueprint's references to ERP software vendors as buyers were a mistake. ERP vendors are platforms and ecosystems, not customers. |

## The plan in one paragraph

UNIFY serves **ERP implementation partners only (SIs / VARs / consultancies)**. ERP software vendors are not customers; they are platforms we connect to and ecosystems we sell through. Partners feel the cost of every manual hour, they implement many products, and there are tens of thousands of them. Win with a narrow, measurable wedge: **an AI-assisted Data Migration & Configuration Workbench for one mid-market ERP** (recommended: Microsoft Dynamics 365 Business Central, with Odoo as a cheap reference adapter), wrapped in a governed project/approval layer and a customer portal. Prove hours saved on 2–3 real projects with 1–2 design partners, then expand to testing, readiness, discovery and a second ERP. The moat is the **implementation knowledge graph** (playbooks, mappings, validation rules, test packs) that compounds with every project, and the partner's own IP captured as reusable packs.
