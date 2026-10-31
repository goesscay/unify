# 08 — AI Agents & Governance

Builds on Part E of the blueprint. The agent list is kept; the change is that agents are **bounded, tool-restricted workers inside durable workflows**, introduced in the order the wedge needs them.

## 1. Agent roster and rollout

| Agent | Job | Tools (allow-list) | Output | Release |
|---|---|---|---|---|
| **Data Mapping Agent** | Map source columns to target fields; propose transforms | `get_entity_schema`, `profile_stats`, `sample_values` (masked), `mapping_memory.search`, `pack.search`, `rule_library.list` | Mapping set with rationale + confidence | MVP |
| **Data Quality Agent** | Explain profiling anomalies; suggest cleansing rules; group exceptions | `profile_stats`, `exceptions.list`, `rule_library.propose` | Cleansing proposals, exception clusters with fix instructions | MVP |
| **Configuration Agent** | Propose setup values from questionnaire/requirements + template; order by dependency | `get_setup_state`, `pack.search`, `config_template.get`, `requirements.list` | Config plan (diff) for approval | MVP |
| **Reconciliation Assistant** | Explain variances; propose root cause | `reconciliations.get`, `migration_records.sample` | Variance explanation | MVP+ |
| **Testing Agent** | Generate requirement-linked tests; drive API test steps | `requirements.list`, `test_pack.get`, `execute_test_step` | Test cases, run evidence | P1 |
| **Readiness Agent** | Evaluate gates; draft the readiness pack | `gates.evaluate`, `evidence.list` | Readiness recommendation (never approval) | P1 |
| **Discovery Agent** | Extract requirements from docs; fit-gap | `documents.read`, `pack.search` | Source-linked requirements | P1 |
| **Planning Agent** | Instantiate and tailor the playbook plan | `playbook.get`, `requirements.list` | Draft plan | P1 |
| **Hypercare Agent** | Classify issues; suggest fixes from knowledge | `issues.read`, `pack.search` | Triage suggestions | P2 |

There is no generic "orchestrator agent" in the MVP: the **Temporal workflow is the orchestrator** and calls agents as steps.

## 2. Agent runtime design

- An agent = `{definition version, system prompt (versioned), model policy, tool allow-list, output JSON schema, budget}`.
- Runs as a Temporal activity: a bounded loop (max steps, max tokens, timeout). Every tool call passes through the **policy engine** and is logged to `agent_steps`.
- **Structured outputs only** (JSON schema validated). Free text appears only inside rationale fields.
- **Read tools are free; write tools don't exist for agents.** Agents produce *proposals*. Writes happen only via workflow activities after approval.
- **Context assembly**: ERP schema + pack snippets (retrieval) + tenant mapping memory + data *profiles and masked samples*, not full datasets.

## 3. AI gateway

- Provider-agnostic routing (task → model tier): small/fast models for classification and column typing; strong reasoning models for mapping, config and fit-gap.
- **PII/sensitive data minimisation**: send column names, statistics and masked/synthetic samples; never send full customer datasets. Configurable per tenant ("no raw values to LLM" mode).
- Zero-data-retention agreements with model providers; regional endpoints where available.
- Prompt and response logging (encrypted, tenant-scoped, retention-limited) for audit and evaluation.
- Cost metering per tenant/project/agent; budgets and alerts.
- Caching for identical schema-mapping requests.

## 4. Evaluation (quality gate before every release)

| Eval set | Content | Metric | MVP target |
|---|---|---|---|
| Mapping gold set | 300+ real-world (anonymised) source files → BC/Odoo targets, including messy headers, local languages, Tally exports | Top-1 accuracy, top-3 recall, calibration (confidence vs. correctness) | ≥ 85% top-1; high-confidence bucket ≥ 97% precise |
| Hallucination | Requests designed to elicit non-existent fields/endpoints | Invalid target rate | 0% (registry-enforced) |
| Config proposals | Questionnaire → expected setup values | Exact-match on critical settings | ≥ 90% |
| Prompt injection | Uploaded docs/spreadsheets containing instructions | Policy violation rate | 0 |
| Regression | All of the above on each prompt/model change | No drop > 2 pts | Blocks release |

Tooling: an eval harness in CI, plus Langfuse (or similar) for traces, datasets and scores.

## 5. Human-in-the-loop policy (default, tenant-configurable)

| Activity | Default policy |
|---|---|
| Profiling, classification | Automatic |
| Mappings with confidence ≥ threshold and origin = memory/template | Bulk-approve by the specialist (one click), still recorded |
| Low-confidence or financial mappings (balances, tax, currency, sign) | Individual review required |
| Transformation rules | Specialist approval; versioned |
| Sandbox load | PM or specialist approval of the load plan |
| Sandbox configuration | Consultant approval of the diff |
| Reconciliation variance | Must be explained and signed off |
| **Any production write** | Two-person approval (partner + customer), time-boxed credential, not in MVP |
| Readiness | Agent recommends; customer sponsor decides |

## 6. Guardrails summary

1. Capability registry = the only source of truth for what can be executed.
2. Uploaded content is untrusted: it is never interpreted as instructions, and it is separated in prompts with clear delimiters and output-schema enforcement.
3. Agents have no network egress except via the gateway and allow-listed tools.
4. Every AI suggestion stores `origin`, `model`, `prompt_version`, `confidence` and `rationale`. Everything is explainable.
5. Learning happens only from **approved** outcomes, within the tenant by default.
6. A kill switch per tenant/agent/adapter.
