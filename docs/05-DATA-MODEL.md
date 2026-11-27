# SDC Studio — Data Model

**Status:** Draft  
**Database baseline:** PostgreSQL  
**Purpose:** Define durable records and relationships required by the approved workflows.

## 1. Data-model rules

- Every project-owned record carries a project scope directly or through a parent relationship.
- Use generated stable primary identifiers (UUID recommended).
- Store timestamps in UTC and render them in the operator's local timezone.
- Store large binary artifacts outside relational rows; persist metadata and references.
- Never store plaintext provider secrets or GitHub App private keys in ordinary tables.
- Use soft deletion only where recovery/audit requirements call for it; do not silently erase audit evidence.
- Constrain state values and relationships in the database.
- Keep user messages, model outputs, tool evidence, and audit records distinguishable.

## 2. Entity relationship overview

    OPERATOR 1---many PROJECT
    PROJECT 1---many REPOSITORY_CONNECTION
    PROJECT 1---many CONVERSATION 1---many MESSAGE 1---many ATTACHMENT
    PROJECT 1---many TASK 1---many TASK_STEP
    TASK 1---many APPROVAL
    TASK 1---many AGENT_RUN 1---many MODEL_INVOCATION
    TASK 1---many TOOL_INVOCATION
    TASK 1---many EXECUTION_WORKSPACE 1---many COMMAND_RUN
    TASK 1---many FILE_CHANGE
    TASK 1---many TEST_RUN 1---many TEST_RESULT
    TASK 1---many ARTIFACT
    PROJECT 1---many DEPLOYMENT_TARGET
    TASK 1---many DEPLOYMENT
    OPERATOR 1---many AUDIT_EVENT

This is conceptual. A specific implementation may normalize or combine tables while preserving ownership and traceability relationships.

## 3. Core entities

### 3.1 Operator and settings

**operators**
- id
- external_identity
- display_name
- created_at
- updated_at

The initial product is private and owner/operator oriented. Do not add public signup, billing, teams, or generalized multi-tenant roles unless the approved foundation changes.

**platform_settings**
- id
- operator_id
- setting_key
- setting_value (non-secret configuration only)
- updated_at

Secret-bearing settings use protected credential storage, not this table.

### 3.2 Projects and repository connections

**projects**
- id
- owner_id
- name
- description
- status
- source_type (blank, local, GitHub)
- created_at
- updated_at
- archived_at (nullable)

**repository_connections**
- id
- project_id
- provider
- repository_owner
- repository_name
- repository_id
- installation_id (nullable)
- default_branch
- selected
- granted_permissions_snapshot
- connected_at
- last_verified_at
- status

Repository metadata must not contain private keys or raw installation tokens.

**repository_snapshots**
- id
- repository_connection_id
- task_id (nullable)
- ref
- commit_sha
- captured_at
- tree_digest (nullable)
- metadata

### 3.3 Conversations and attachments

**conversations**
- id
- project_id
- title
- created_at
- updated_at

**messages**
- id
- conversation_id
- role (operator, assistant, system-summary where applicable)
- content
- mode
- autonomy_level
- created_at
- provider_invocation_id (nullable)

**attachments**
- id
- message_id
- artifact_id
- original_filename
- media_type
- size_bytes
- content_hash
- validation_status

Attachment bytes live in artifact storage. Validate type and size before model use.

### 3.4 Tasks and plans

**tasks**
- id
- project_id
- conversation_id
- created_by
- request_message_id
- mode
- autonomy_level
- status
- summary
- baseline_commit_sha (nullable)
- working_branch (nullable)
- started_at
- finished_at
- created_at
- updated_at

**task_plans**
- id
- task_id
- version
- plan_content
- acceptance_criteria
- created_at
- approved_at (nullable)

**task_steps**
- id
- task_id
- plan_version
- sequence
- title
- description
- status
- depends_on
- started_at
- finished_at
- result_summary

### 3.5 Agents, models, and provider usage

**agent_runs**
- id
- task_id
- task_step_id
- role
- status
- started_at
- finished_at
- result_summary

**provider_configurations**
- id
- owner_id
- provider_key
- display_name
- credential_reference (reference to encrypted secret storage)
- enabled
- data_terms_acknowledged_at (nullable)
- created_at
- updated_at

**model_catalog**
- id
- provider_configuration_id
- provider_model_id
- display_name
- capabilities
- context_limit (nullable/known value only)
- free_tier_status
- enabled
- last_checked_at

**model_invocations**
- id
- agent_run_id
- provider_configuration_id
- model_catalog_id
- request_metadata_redacted
- response_metadata
- input_tokens (nullable)
- output_tokens (nullable)
- latency_ms
- status
- error_code
- created_at

Do not persist raw sensitive prompts or responses in usage telemetry unless they are already part of the authorized conversation/task record and applicable data policy permits it.

### 3.6 Tool operations and approvals

**tool_invocations**
- id
- task_id
- task_step_id
- operation_id (unique)
- tool_name
- tool_version
- permission_category
- scope_metadata
- arguments_redacted
- status
- started_at
- finished_at
- exit_code (nullable)
- result_summary
- error_code

**approvals**
- id
- task_id
- tool_invocation_id
- requested_operation
- scope_summary
- impact_summary
- status (pending, approved, rejected, expired)
- requested_at
- decided_at
- decision_actor

Approval applies only to the specified operation and scope.

### 3.7 Workspaces, commands, and changes

**execution_workspaces**
- id
- project_id
- task_id
- runner_id
- workspace_path_reference
- repository_ref
- baseline_commit_sha
- working_branch
- status
- resource_profile
- created_at
- destroyed_at

Do not expose host paths or sensitive environment information to untrusted models.

**command_runs**
- id
- workspace_id
- tool_invocation_id
- command_redacted
- working_directory
- started_at
- finished_at
- exit_code
- stdout_artifact_id
- stderr_artifact_id
- timeout_status

**file_changes**
- id
- task_id
- workspace_id
- path
- change_type
- before_hash
- after_hash
- diff_artifact_id
- recorded_at

### 3.8 Tests and browser evidence

**test_runs**
- id
- task_id
- workspace_id
- test_type
- command_or_profile
- status
- started_at
- finished_at
- summary
- report_artifact_id

**test_results**
- id
- test_run_id
- test_identifier
- status
- duration_ms
- failure_summary
- evidence_artifact_id

**browser_sessions**
- id
- task_id
- workspace_id
- preview_origin
- browser_name
- viewport
- status
- started_at
- finished_at

**browser_events**
- id
- browser_session_id
- event_type (console, page_error, request_failed, response_status, navigation, screenshot, trace)
- occurred_at
- url_redacted
- status_code
- message_redacted
- artifact_id

### 3.9 Artifacts, deployment, and audit

**artifacts**
- id
- project_id
- task_id (nullable)
- artifact_type
- storage_key
- media_type
- size_bytes
- content_hash
- retention_until (nullable)
- created_at

**deployment_targets**
- id
- project_id
- provider
- target_name
- configuration_reference
- status
- created_at
- updated_at

**deployments**
- id
- task_id
- deployment_target_id
- provider_deployment_id
- status
- deployment_url
- started_at
- finished_at
- logs_artifact_id
- verification_summary

Production credentials remain in protected secret storage; records contain references only.

**audit_events**
- id
- operator_id
- project_id (nullable)
- task_id (nullable)
- operation_id (nullable)
- event_type
- actor_type
- actor_reference
- target_reference
- details_redacted
- created_at

Audit events are append-only for ordinary application operations. Corrections are recorded as new events rather than silently rewriting history.

## 4. Status values

Task states: Draft, Planning, AwaitingApproval, Queued, Running, Verifying, Repairing, Completed, CompletedWithIssues, Blocked, Failed, Cancelled.

Test states: Passed, Failed, Skipped, TimedOut, NotRun.

Approval states: Pending, Approved, Rejected, Expired.

Provider/model states: Enabled, Disabled, Unavailable, RateLimited, ConfigurationError.

## 5. Indexes and integrity

- Index foreign keys and common project/task timeline queries.
- Unique index on tool operation ID.
- Unique index on provider configuration and provider model ID.
- Use constraints for allowed states and nonnegative sizes/durations.
- Use transactions when changing task state and persisting related events.
- Prevent deletion of parent records while required audit/evidence references remain.

## 6. Retention and privacy

- Keep only data necessary for project history, debugging, audit, and owner-selected retention.
- Keep secrets outside ordinary relational records.
- Redact credentials, cookies, authorization headers, and sensitive query parameters before persisting logs.
- Restrict artifact retrieval by project/task authorization.
- Do not silently upload private code, screenshots, or logs to third-party AI providers.

## 7. Migration policy

Schema migrations are versioned, reviewed, and reversible where practical. Never run destructive production migrations without backup/recovery planning and explicit approval. Detailed migration scripts belong to implementation phases.

## 8. Scope note

This is the Phase 1 logical data model. Column types, full indexes, encryption implementation, and migration files are finalized during implementation without changing approved data ownership and security principles.
