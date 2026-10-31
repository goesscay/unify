# 09 — Security, Compliance & Trust

UNIFY holds end-customers' master and financial data and credentials to their ERPs. For a partner, trust is a buying criterion, not a feature. This document extends Part G of the blueprint.

## 1. Threat model (top risks)

| Threat | Control |
|---|---|
| Cross-tenant data leak (partner A sees partner B; customer X sees customer Y) | RLS on every table; scoped object-storage prefixes; tenant-scoped vector search; automated isolation tests in CI; pen tests |
| ERP credential theft → fraudulent writes | Vault storage; credentials never returned to the UI; least-privilege app registrations; per-environment credentials; short-lived tokens; egress allow-lists |
| Malicious upload (macro, zip bomb, parser exploit) | Type allow-list, size limits, ClamAV, sandboxed parsing workers, no macro execution |
| Prompt injection via uploaded docs/data | Untrusted-content handling, no write tools for agents, schema-validated outputs, policy engine |
| Insider/support misuse | Just-in-time support access with customer consent, recorded; admin actions audited |
| Wrong or destructive writes to ERP | Sandbox-first, approvals, dry runs, idempotency, read-back verification, production two-person rule |
| Data exfiltration through LLM providers | Data minimisation, zero-retention terms, regional endpoints, tenant "no raw values" mode |
| Account takeover | MFA mandatory for internal users; SSO; anomaly alerts |

## 2. Control baseline (MVP)

- OIDC auth, MFA, session management, RBAC + scoped role assignments enforced server-side
- TLS 1.2+; AES-256 at rest (RDS, S3 with KMS); per-tenant KMS keys for Enterprise
- Append-only, hash-chained audit log; tamper-evident exports
- Secrets manager; rotation; no secrets in code (secret scanning in CI)
- SAST, dependency and container scanning; IaC scanning; signed images
- Backups: PITR on Postgres, versioned S3, quarterly restore tests; RPO ≤ 15 min, RTO ≤ 4 h
- Logging without sensitive payloads; PII redaction in logs
- Vulnerability disclosure policy; incident response runbook; 72-hour breach notification readiness

## 3. Compliance roadmap

| Item | Why | When |
|---|---|---|
| **DPDP Act 2023 (India)** readiness: consent and notice, data processing agreement, breach process | India-hosted customer data | MVP |
| **GDPR / UK GDPR**: DPA, SCCs, subprocessors list, DPIA | UK/EU partners and their customers | Before the first EU/UK customer |
| **SOC 2 Type I → Type II** | Main ask from US/UK partners and their enterprise customers | Type I by ~month 9–10; Type II ~month 15–18 (use Vanta/Drata/Sprinto) |
| **ISO 27001** | Asks from India/Middle East enterprise and SIs | Year 2 |
| Microsoft AppSource / partner listing security review | Distribution in the BC ecosystem | With the BC GA adapter |
| VAPT by a CERT-In empanelled auditor | Indian enterprise customers often require it | Before GA |

## 4. Trust features that sell

- Public **Trust Center** page (subprocessors, certifications, uptime)
- Customer-visible **audit trail** and **evidence pack** for every migration
- Data residency choice (India / UK-EU, later US)
- Bring-your-own-model-key and private deployment (Enterprise)
- Clear data-deletion certificate at project archive
