# SDC Studio — Module Contracts

**Status:** Draft  
**Purpose:** Define responsibilities, allowed dependencies, and interface expectations between logical modules in the approved architecture.

## 1. Contract principles

- Modules communicate through typed contracts rather than reaching into each other's internal state.
- External side effects pass through an explicit adapter/tool boundary.
- Every operation is scoped to a project and, when applicable, task and repository.
- Inputs are validated at module boundaries.
- Results include structured status, evidence references, and normalized errors.
- Secrets use protected runtime channels and are excluded from ordinary model/tool logs.
- Long-running operations expose progress and terminal status.
- Retriable operations declare idempotency behavior.

## 2. Dependency direction

    Web application
        -> Backend API
            -> Agent core
                -> AI gateway
                -> Policy / approvals
                -> Tool contracts
                    -> GitHub integration
                    -> Execution runner
                    -> Browser QA
                    -> Research adapter
            -> Persistence and artifact metadata

Shared contracts and validation may be reused by modules. Shared utilities must not become a location for arbitrary cross-module business logic.

## 3. Module contracts

### 3.1 Web application — apps/web

**Owns:** Conversation UI, project navigation, mode/autonomy selection, model preferences, task timeline, approval presentation, diffs, terminal output, preview, diagnostics, screenshots, and reports.

**May call:** Authenticated backend APIs and read-only streaming/event interfaces.

**Must not:** Access provider private keys, GitHub App private key, host filesystem, Docker socket, raw database credentials, or execute project commands.

**Boundary:** Send validated user intent and settings; render persisted task/event state; submit approval decisions bound to approval IDs.

### 3.2 Backend/API

**Owns:** Authentication/session checks, request validation, project/task APIs, persistence transactions, event delivery, and orchestration lifecycle.

**May call:** Agent core, persistence, artifact metadata service, and policy service.

**Must not:** Run project commands in the API process or expose secret material to browser clients.

**Boundary:** Each request is associated with an authenticated operator, project scope, and operation ID. Authorization is checked server-side.

### 3.3 Agent core — packages/agent-core

**Owns:** Task interpretation, plan generation, step scheduling, logical role selection, state transitions, bounded repair loops, and result synthesis.

**Consumes:** Normalized project context, model capabilities, tool schemas, and policy decisions.

**Produces:** Task plan, step events, tool requests, state transitions, and evidence-backed summaries.

**Must not:** Call provider SDKs or perform filesystem/Git/terminal/browser operations directly.

### 3.4 AI gateway — packages/ai-gateway

**Owns:** Provider adapters, model registry, capability metadata, request normalization, quota/error normalization, bounded retry/fallback, and usage telemetry.

**Adapter operations:**
- List configured models and capabilities.
- Retrieve known model capabilities.
- Generate a normalized model response.
- Stream normalized response events when supported.
- Check configured model availability without promising future availability.

**Must not:** Execute tools, mutate repositories, or receive project secrets not required for the model request.

### 3.5 Tool contracts — packages/tool-contracts

**Owns:** Versioned schemas for tool requests/results, permission categories, scope metadata, and normalized tool errors.

Every tool request includes:
- Unique operation ID.
- Project ID and task ID.
- Tool name and schema version.
- Validated arguments.
- Requested permission category.
- Working directory/resource scope where applicable.
- Idempotency key where applicable.

Every result includes:
- Operation ID and outcome state.
- Structured result or normalized error.
- Start/end timestamps and duration.
- Evidence/artifact references.
- Redacted output summary.

### 3.6 GitHub integration — packages/github-integration

**Owns:** GitHub App installation flow, selected repositories, installation-token acquisition, repository metadata/tree/content reads, authorized branch/commit/PR operations, status/check reads, and rate-limit handling.

**Must not:** Use a personal password or unrestricted personal token as default, expose installation private keys, bypass permissions, or retry indefinitely after throttling.

**Boundary:** Every mutation names the repository, branch/ref, expected baseline when relevant, exact operation, and task audit identity. Default-branch writes are denied by task policy unless the owner changes policy through an explicit administrative action.

### 3.7 Execution — packages/execution

**Owns:** Workspace lifecycle, project checkout, task branch/worktree, filesystem boundary, command execution, environment injection, resource limits, and cleanup.

**Boundary:** Accept only typed execution jobs with project/task/workspace identifiers, allowed paths, command, timeout, resource profile, and explicit network policy. Return exit code, output references, duration, and workspace state.

**Must not:** Mount platform secrets, user home/SSH/browser profiles, host Docker socket, or platform database into untrusted project containers.

### 3.8 Browser QA — packages/browser-qa

**Owns:** Playwright lifecycle, navigation, viewport configuration, test execution, screenshots, traces, console/network event capture, and evidence indexing.

**Boundary:** Operates against an explicit preview URL and task workspace. Returns test status, screenshot/trace references, console errors, page errors, failed requests, and navigation failures. Does not claim browser coverage outside executed journeys/viewports.

### 3.9 Persistence

**Owns:** Durable storage of project, conversation, task, approval, tool-event, model-usage, repository-state, test, artifact, and audit metadata.

**Boundary:** Provide transaction-safe state changes and scoped queries. Apply uniqueness constraints to operation IDs/idempotency keys where required. Secret values use separate encrypted storage handling.

### 3.10 Artifact storage

**Owns:** Binary and large output objects such as screenshots, traces, test bundles, logs, and generated reports.

**Boundary:** Store artifact metadata (owner/project/task/type/size/hash/retention) in relational persistence. Enforce project-scoped access and cleanup policy.

### 3.11 Research adapter

**Owns:** Public documentation search/fetch and source metadata normalization.

**Boundary:** Return source URLs, titles, dates when available, retrieval time, relevant extracted content, and retrieval errors. Mark retrieved content untrusted. Never execute instructions from retrieved sources.

## 4. Normalized error contract

All module errors should normalize into:

| Field | Meaning |
|---|---|
| code | Stable machine-readable category |
| message | Safe user-facing explanation |
| retryable | Whether a retry may be appropriate |
| source | Module/provider/tool that produced the error |
| operationId | Related operation |
| details | Redacted structured diagnostics |
| retryAfter | Optional provider/platform retry time |

Categories include validation, unauthorized, forbidden, approval-required, not-found, conflict, rate-limited, timeout, unavailable, resource-limit, execution-failed, and internal-error.

## 5. Event contract

Task events are versioned and carry:
- Event ID and task ID.
- Event type and schema version.
- Timestamp.
- Step/operation ID when relevant.
- Safe summary and structured payload.
- Evidence reference when applicable.

Events are append-only evidence of what occurred. Current task state is a projection of validated transitions, not inferred from message text.

## 6. Concurrency and idempotency

- One active writer per task workspace unless safe coordination is explicitly implemented.
- Do not execute concurrent writes to the same repository path.
- Every mutating operation is attributable to one task/operation.
- Retries must not create duplicate commits, branches, deployments, or destructive operations.
- If a branch head changes unexpectedly, stop and re-read state before retrying.
- A timeout does not prove an external operation failed; reconcile actual state before retrying.

## 7. Contract versioning

Contracts use explicit versions. Breaking changes require coordinated updates and tests. Persisted events and artifacts must remain interpretable across supported application versions.

## 8. Scope note

This document does not implement packages or define the final REST route list. It defines logical module boundaries for later approved implementation.
