# 06 — Data Model

Extends Part D of the SME blueprint: **a product company is a tenant; each tenant registers one or more products and onboards many customers; each customer can have multiple implementation projects.** This document adds environment-scoped connections, versioned capability registries, playbooks and knowledge packs, record-level lineage, and the policy and evidence tables.

## 1. Hierarchy

```
tenant (ERP product company)
 ├── users / memberships / roles / role_assignments
 ├── products (the tenant's own ERP/product)
 │    ├── product_versions → modules → entities → fields
 │    ├── adapters (per product version range) → capability registry (versioned)
 │    └── knowledge_packs / playbooks / config_templates / rule_sets / test_packs (versioned)
 └── customers (the product company's customers)
      └── projects (customer × product)
           ├── environments (sandbox, production) → connections (secret ref)
           ├── phases / tasks / milestones / risks / decisions
           ├── requirements / questionnaire_responses
           ├── files → file_sheets → data_profiles
           ├── migration_objects → mapping_versions → field_mappings → runs → batches → records (lineage) → reconciliations
           ├── configuration_jobs → configuration_items (before/after)
           ├── integration_connections → integration_runs
           ├── test_cases → test_runs → defects
           ├── readiness_gates → evidence
           ├── approvals / exceptions
           └── agent_runs → agent_steps (tool calls) → usage_records
audit_events (append-only, all levels)
```

## 2. Core tables (PostgreSQL)

Conventions: UUID v7 primary keys; `tenant_id` on **every** tenant-owned table (denormalised for RLS); `created_at`, `updated_at`, `created_by`; soft delete via `archived_at`; JSONB only for variable metadata (blueprint rules).

### A. Tenancy and access
- `tenants(id, name, region, plan_id, status[pending|active|suspended], settings jsonb)`: every tenant is an ERP product company; activation requires UNIFY approval
- `users(id, email, name, status, idp_subject)`
- `memberships(tenant_id, user_id, kind[internal|customer], customer_id null, status)`
- `roles(id, tenant_id null, name, is_system)`; `role_permissions(role_id, permission)`
- `role_assignments(tenant_id, user_id, role_id, scope_type[tenant|product|customer|project|environment], scope_id)`
- `api_keys(tenant_id, name, hash, scopes, expires_at)`

### B. Products, adapters and knowledge (versioned)
- `products(id, tenant_id, name, category[erp|wms|crm|…], hosting_model, status)`
- `product_versions(id, tenant_id, product_id, version, released_at, status)`
- `product_modules(id, tenant_id, product_version_id, code, name)`
- `product_entities(id, tenant_id, product_version_id, module_id, code, name, load_order, schema jsonb)`
- `product_fields(id, tenant_id, entity_id, code, data_type, required, ref_entity_id, constraints jsonb)`
- `adapters(id, tenant_id, product_id, version, version_range, runtime[python|http], status[draft|certified|deprecated], certified_at)`
- `capabilities(id, tenant_id, adapter_id, operation, entity_id null, environments text[], approved_by, version)`: **the source of truth for agent execution**
- `knowledge_packs(id, tenant_id null, scope[global|tenant], product_id null, kind[implementation_rules|localisation|industry|source_system], version, status)`
- `knowledge_documents(pack_id, tenant_id, title, source_uri, checksum)`; `knowledge_chunks(document_id, tenant_id, content, embedding vector)`
- `playbooks(id, tenant_id, product_id, name, version, definition jsonb, status)`
- `config_templates(id, tenant_id, product_id, version, items jsonb)`
- `rule_sets(id, tenant_id, product_id, entity_id, version, rules jsonb)`
- `mapping_memory(tenant_id, product_id, source_system, source_field_signature, target_entity_id, target_field_id, approvals_count, last_approved_at)`: the learning store

### C. Customers and projects
- `customers(id, tenant_id, name, industry, country, legal_entities jsonb, branches jsonb)`
- `projects(id, tenant_id, customer_id, product_id, product_version_id, playbook_id, playbook_version, name, stage, health, target_go_live, status)`
- `environments(id, tenant_id, project_id, kind[sandbox|production|test], connection_id)`
- `connections(id, tenant_id, adapter_id, base_url, instance_ref, secret_ref, status, last_checked_at)`
- `phases`, `tasks(… owner_user_id, owner_side[internal|customer], due_at, status)`, `task_dependencies`, `milestones`, `risks`, `decisions`
- `requirements(id, tenant_id, project_id, code, text, module_id, fit[standard|config|extension|gap], source_ref, confidence, status)`

### D. Files and migration
- `files(id, tenant_id, project_id, storage_key, checksum_sha256, mime, size, scan_status, uploaded_by)`
- `file_sheets(file_id, name, row_count, header_row, schema jsonb)`; `data_profiles(sheet_id, column, stats jsonb)`
- `migration_objects(id, tenant_id, project_id, target_entity_id, source_sheet_ids, status, load_order)`
- `mapping_versions(id, tenant_id, migration_object_id, version, status, approved_by, approved_at)`
- `field_mappings(mapping_version_id, source_expr, target_field_id, transform_rule_ids, rationale, confidence, origin[ai|template|memory|user])`
- `migration_runs(id, tenant_id, migration_object_id, mapping_version_id, environment_id, kind[dry_run|load|rehearsal], status, stats jsonb)`
- `migration_batches(run_id, seq, idempotency_key, status, attempts, error)`
- `migration_records(run_id, source_file_id, source_row, source_hash, transformed jsonb, target_id, status[loaded|rejected|skipped], errors jsonb)`: lineage, partitioned by run; very large runs spill to Parquet in S3
- `reconciliations(run_id, check_type, expected, actual, variance, status, signed_off_by)`

### E. Configuration, integration, testing, readiness
- `configuration_jobs(id, tenant_id, project_id, environment_id, template_id, status)`; `configuration_items(job_id, setting_path, proposed, approved, before, after, verified)`
- `integration_connections(id, tenant_id, project_id, system_name, kind[rest|webhook|sftp], secret_ref, status)`; `integration_mappings`; `integration_runs`
- `test_cases(id, tenant_id, project_id, requirement_ids uuid[], steps jsonb, expected jsonb, automation[api|manual])`; `test_runs`; `defects(severity, status)`
- `readiness_gates(id, tenant_id, project_id, code, rule jsonb, status, evidence_ids uuid[])`

### F. Governance and AI
- `policies(tenant_id, action_type, environment_kind, condition jsonb, approver_roles, min_approvers)`
- `approvals(id, tenant_id, project_id, subject_type, subject_id, policy_id, status, decided_by, decided_at, comment, payload_hash)`
- `exceptions(id, tenant_id, project_id, subject_type, subject_id, assignee_id, owner_side, severity, status, due_at)`
- `evidence(id, tenant_id, project_id, kind, storage_key, hash, created_by_run)`
- `agent_definitions(code, version, model_policy, tools jsonb, prompt_ref)`
- `agent_runs(id, tenant_id, project_id, agent_code, agent_version, input_ref, output_ref, status, tokens_in, tokens_out, cost)`; `agent_steps(run_id, seq, tool, args_hash, result_ref, policy_decision)`
- `audit_events(id, tenant_id, actor_type, actor_id, action, subject_type, subject_id, ip, payload jsonb, prev_hash, hash, at)`: append-only, hash-chained
- `notifications`, `usage_records(tenant_id, project_id, meter, quantity, at)`, `subscriptions`, `invoices`

## 3. Rules (blueprint's ten rules, made concrete)

1. RLS policy on every tenant table: `tenant_id = current_setting('app.tenant_id')::uuid`. Customer users get additional policies on `customer_id`/`project_id`.
2. The application DB role cannot bypass RLS; schema migrations use a separate role.
3. Original files are never modified; source, transformation and target layers are stored separately.
4. Secrets are referenced (`secret_ref`), never stored.
5. Product schemas, capabilities, playbooks, mappings, rules and agent definitions are versioned; projects pin versions.
6. Retention configurable per tenant (90 days–7 years); audit events retained ≥ 7 years unless the contract says otherwise.
7. Full project export (JSON + Parquet + files) for portability and offboarding.
8. Learning across tenants only via **global packs** built from non-confidential knowledge, or from anonymised statistics with tenant opt-in.
