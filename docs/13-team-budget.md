# 13 — Team, Budget & Operating Plan

## 1. Team plan

| Role | Phase 0 (wk 0–8) | Phases 1–4 (mo 3–9) | GA (mo 9–15) |
|---|---|---|---|
| Product owners (product, implementation domain, business) | 2 | 2 | 2 |
| Tech lead (backend/platform, Python, Temporal) | 1 (hire first) | 1 | 1 |
| AI/data engineer (mapping, evals, rules engine) | 0.5 (contract) | 1 | 2 |
| Full-stack engineer (Next.js, grids, portals) | — | 1 | 2 |
| Integration engineer (adapters: Odoo reference + design partner's product) | 0.5 (contract) | 1 | 2 |
| QA / automation | — | 1 (from month 5) | 1 |
| ERP functional consultant (implementation methodology; packs, rules, tests; works with design partner's team) | Advisor | 1 (part-time → full) | 1–2 |
| UX designer | Part-time | Part-time | 1 |
| DevOps/SRE + security | Tech lead | Part-time contractor | 1 |
| Sales | Product owners | Product owners | 1 enterprise AE + 1 solutions engineer |
| **Headcount (FTE)** | **~4** | **~8–9** | **~14–16** |

**A key addition to the blueprint:** a **hands-on ERP functional consultant**. The knowledge packs, validation rules and test packs are the moat, and engineers cannot author them alone.

## 2. Budget (India-based team, INR)

| Category | Phase 0 (2 mo) | Phases 1–4 (7 mo) | Notes |
|---|---|---|---|
| People | ₹8–12 L | ₹40–55 L | Blended; product owners' salaries excluded |
| Cloud (AWS), Temporal Cloud, tooling | ₹0.5–1 L | ₹4–7 L | Credits: AWS Activate, Microsoft for Startups, model-provider credits |
| AI model usage | ₹0.3 L | ₹2–4 L | Evals + pilots |
| Sandboxes / test environments | ₹0.3 L | ₹1–2 L | Odoo self-hosted (free); design partner provides its product sandbox |
| Security (pen test, VAPT), compliance tooling | — | ₹4–7 L | SOC 2 tooling begins ~month 8 |
| Legal, company, trademark, contracts | ₹1–2 L | ₹2–3 L | |
| Design, website, travel/events | ₹0.5–1 L | ₹3–5 L | |
| **Total** | **₹11–17 L** | **₹56–84 L** | **≈ ₹67 L–1 Cr to MVP launch (~9 months)** |

This is higher than the blueprint's ₹36–58 L because it adds a functional consultant, security testing, Phase 0 and a realistic ramp. A lean version, with the product owners covering more roles, can come in nearer ₹45–55 L by deferring QA and design hires.

**Funding implication:** a pre-seed of roughly **US$300–600k** (or ₹2.5–5 Cr) gives 15–18 months of runway through GA and first ARR. Raise after G0 (design partners signed), which strengthens the round.

## 3. Operating rhythm

- Weekly: product/engineering planning; design-partner check-in
- Bi-weekly: sprint demo (design partners invited)
- Monthly: metrics review (pipeline, pilot KPIs, AI quality, burn)
- Quarterly: strategy, roadmap, and a board/advisor update

## 4. Tooling

GitHub (code, issues, projects), Linear or GitHub Projects (backlog), Figma (design), Notion/Docs (specs), Slack, HubSpot (CRM), Vanta/Drata/Sprinto (compliance), Langfuse (LLM observability), Sentry, Grafana/Datadog.
