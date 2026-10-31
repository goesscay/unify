# 06 — Data Model

Extends Part D of the blueprint. Changes from the blueprint: a `customer`/client-account layer under the partner tenant, environment-scoped connections, playbooks and knowledge packs as versioned assets, record-level lineage, and the evidence/policy tables.

## 1. Hierarchy

```
tenant (partner or vendor)
 ├── users / memberships / roles
 ├── erp_products (BC, Odoo, NetSuite…) ← global catalogue + tenant enablement
 ├── knowledge_packs / playbooks / config_templates / rule_sets / test_packs (versioned; global, vendor or tenant scope)
 └── customers (end-client accounts)
      └── projects
           ├── environments (sandbox, production) → erp_connections (secret ref)
           ├── phases / tasks / milestones / risks / decisions
           ├── requirements / questionnaire_responses
           ├── files → file_sheets → data_profiles
           ├── migration_objects → mappings → rules → runs → batches → records(lineage) → reconciliations
           ├── configuration_jobs → configuration_items (before/after)
           ├── test_cases → test_runs → defects
           ├── readiness_gates → evidence
           ├── approvals / exceptions
           └── agent_runs → agent_steps (tool calls) → usage_records
audit_events (append-only, all levels)
```

## 2. Core tables (PostgreSQL)

Conventions: UUID v7 primary keys; `tenant_id` on **every** tenant-owned table (denormalised for RLS); `created_at`, `updated_at`, `created_by`; soft-delete via `archived_at`; JSONB only for variable metadata.

### A. Tenancy and access
- `tenants(id, type[partner|vendor], name, region, plan_id, status, settings jsonb)`
- `users(id, email, name, status, idp_subject)`
- `memberships(tenant_id, user_id, kind[internal|customer], customer_id null, status)`
- `roles(id, tenant_id null, name, is_system)`; `role_permissions(role_id, permission)`
- `role_assignments(tenant_id, user_id, role_id, scope_type[tenant|customer|project|environment], scope_id)`
- `api_keys(tenant_id, name, hash, scopes, expires_at)`

### B. Catalogue and knowledge (versioned)
- `erp_products(id, code, name, vendor, versions jsonb)`: global
- `adapters(id, erp_product_id, version, status, capabilities jsonb)`
- `erp_entities(adapter_id, code, name, load_order, schema jsonb)`; `erp_fields(entity_id, code, type, required, ref_entity, constraints jsonb)`
- `knowledge_packs(id, scope[global|vendor|tenant], owner_tenant_id, erp_product_id, kind[localisation|industry|methodology], version, status)`
- `knowledge_documents(pack_id, title, source_uri, checksum)`; `knowledge_chunks(document_id, tenant_id null, content, embedding vector)`
- `playbooks(id, scope, owner_tenant_id, erp_product_id, name, version, definition jsonb, status)`
- `config_templates(id, scope, owner_tenant_id, erp_product_id, version, items jsonb)`
- `rule_sets(id, scope, owner_tenant_id, entity_code, version, rules jsonb)`
- `mapping_memory(tenant_id, erp_product_id, source_system, source_field_signature, target_entity, target_field, approvals_count, last_approved_at)`: the learning store

### C. Customers and projects
- `customers(id, tenant_id, name, industry, country, legal_entities jsonb)`
- `projects(id, tenant_id, customer_id, erp_product_id, playbook_id, playbook_version, name, stage, health, target_go_live, status)`
- `environments(id, tenant_id, project_id, kind[sandbox|production|test], erp_connection_id)`
- `erp_connections(id, tenant_id, adapter_id, base_url, company_ref, secret_ref, status, last_checked_at)`
- `phases, tasks(… owner_user_id, owner_side[partner|customer], due_at, status), task_dependencies, milestones, risks, decisions`
- `requirements(id, tenant_id, project_id, code, text, module, fit[standard|config|extension|gap], source_ref, confidence, status)`

### D. Files and migration
- `files(id, tenant_id, project_id, storage_key, checksum_sha256, mime, size, scan_status, uploaded_by)`
- `file_sheets(file_id, name, row_count, header_row, schema jsonb)`; `data_profiles(sheet_id, column, stats jsonb)`
- `migration_objects(id, tenant_id, project_id, target_entity, source_sheet_ids, status, load_order)`
- `mapping_versions(id, migration_object_id, version, status, approved_by, approved_at)`
- `field_mappings(mapping_version_id, source_expr, target_field, transform_rule_ids, rationale, confidence, origin[ai|template|memory|user])`
- `migration_runs(id, tenant_id, migration_object_id, mapping_version_id, environment_id, kind[dry_run|load|rehearsal], status, stats jsonb)`
- `migration_batches(run_id, seq, idempotency_key, status, attempts, error)`
- `migration_records(run_id, source_file_id, source_row, source_hash, transformed jsonb, target_id, status[loaded|rejected|skipped], errors jsonb)`: **lineage**; partitioned by run; large runs may spill to Parquet in S3 with an index here
- `reconciliations(run_id, check_type, expected, actual, variance, status, signed_off_by)`

### E. Configuration, testing, readiness
- `configuration_jobs(id, project_id, environment_id, template_id, status)`; `configuration_items(job_id, setting_path, proposed, approved, before, after, verified)`
- `test_cases(id, project_id, requirement_ids uuid[], steps jsonb, expected jsonb, automation[api|manual])`; `test_runs`; `defects(severity, status)`
- `readiness_gates(project_id, code, rule jsonb, status, evidence_ids uuid[])`

### F. Governance and AI
- `policies(tenant_id, action_type, environment_kind, condition jsonb, approver_roles, min_approvers)`
- `approvals(id, tenant_id, project_id, subject_type, subject_id, policy_id, status, decided_by, decided_at, comment, payload_hash)`
- `exceptions(id, project_id, subject_type, subject_id, assignee_id, owner_side, severity, status, due_at)`
- `evidence(id, project_id, kind, storage_key, hash, created_by_run)`
- `agent_definitions(code, version, model_policy, tools jsonb, prompt_ref)`
- `agent_runs(id, tenant_id, project_id, agent_code, agent_version, input_ref, output_ref, status, tokens_in, tokens_out, cost)`; `agent_steps(run_id, seq, tool, args_hash, result_ref, policy_decision)`
- `audit_events(id, tenant_id, actor_type, actor_id, action, subject_type, subject_id, ip, payload jsonb, prev_hash, hash, at)`: append-only, hash-chained
- `usage_records(tenant_id, project_id, meter, quantity, at)`; `subscriptions`, `invoices` (from the billing provider)

## 3. Rules

1. RLS policy on every tenant table: `tenant_id = current_setting('app.tenant_id')::uuid`. Customer users get additional policies on `customer_id`/`project_id`.
2. The application DB role cannot bypass RLS; migrations use a separate role.
3. Original files are never modified; derived data is stored separately (source → transformation → target layers, as in the blueprint).
4. Secrets are referenced (`secret_ref`), never stored.
5. Retention: the tenant chooses 90 days to 7 years for project data; files are purged on project archive plus the retention period; audit events are retained ≥ 7 years unless contractually changed.
6. Data export: full project export (JSON + Parquet + files) for portability and offboarding.
7. Cross-tenant learning only via **global packs** curated by UNIFY from non-confidential knowledge (ERP docs), never from tenant data without opt-in.
