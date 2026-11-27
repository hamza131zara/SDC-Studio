# SDC Studio — System Architecture

**Status:** Draft  
**Architecture baseline:** Approved Phase 0 Foundation  
**Style:** Modular monolith with isolated execution workers

## 1. Architecture objectives

- Keep the initial product practical to build and operate privately.
- Separate model reasoning from controlled tool execution.
- Isolate untrusted project execution from platform services and credentials.
- Make providers, repository access, runner, and browser capabilities replaceable behind typed contracts.
- Persist task state and evidence so the operator can understand and recover work.
- Avoid premature microservices and unnecessary managed-service costs.

## 2. Logical architecture

    Operator
       |
       v
    Web application
       |
       v
    Backend API ---- Persistence (projects, chats, tasks, events, approvals)
       |
       v
    Agent orchestrator ---- Permission and approval policy
       |                         |
       |                         +---- Tool gateway
       |                                   |
       +---- Model gateway                 +---- GitHub adapter
                 |                         +---- Execution runner
                 +---- Cloud AI providers  +---- Research adapter
                 +---- Optional local     +---- Browser/QA runner
                       model runtime                 |
                                                     v
                                            Isolated project workspace
                                                     |
                                                     v
                                            Screenshots, traces,
                                            logs and test evidence
                                                     |
                                                     v
                                                Artifact storage

The diagram shows logical responsibilities, not a mandatory network topology. Initially, the web/API and local runner may run on the operator's computer. The runner remains a distinct security boundary even when it shares a host.

## 3. Deployment topology

### 3.1 Initial local-first topology

- Operator opens the web application in a local browser.
- Web/API manages product state, conversations, project records, and task orchestration.
- Local runner manages project workspaces, Git operations, project commands, and browser sessions.
- Model inference may use configured external APIs or optional local models.
- PostgreSQL and artifact storage may run locally during initial development.

The web/API process must not execute untrusted project commands directly. It sends typed task operations to the runner after authorization.

### 3.2 Future hosted topology

A hosted web/API may later coordinate authenticated remote runners. This is an extension, not an initial implementation commitment. A remote runner authenticates to the platform, receives only scoped job credentials, and never receives platform master secrets.

## 4. Component responsibilities

### 4.1 Web application

Owns:
- Conversations and chat presentation.
- Project and repository selection UI.
- Operating mode, autonomy, and model selection.
- Task timeline and approval prompts.
- File diffs, terminal output, preview, screenshots, QA results, and reports.

Must not:
- Store provider or GitHub private credentials in browser-accessible state.
- Execute arbitrary repository commands.
- Make policy decisions solely in client-side code.

### 4.2 Backend/API

Owns:
- Authenticated product API.
- Project/conversation/task persistence.
- Settings and permission policy evaluation.
- Artifact metadata and access checks.
- Streaming task events to the UI.
- Orchestrator lifecycle.

Must validate each operation server-side and avoid trusting a client-supplied project/repository identity without checking ownership and authorization.

### 4.3 Agent orchestrator

Owns:
- Task interpretation and decomposition.
- Specialist-role selection.
- Plan and acceptance criteria.
- Step scheduling, state transitions, retries, and bounded repair loops.
- Tool selection through the tool gateway.
- Synthesis of tool evidence into a final response/report.

It does not directly access the host filesystem, raw provider secrets, GitHub private keys, Docker daemon, or browser profile.

### 4.4 Model gateway

Owns:
- Provider adapters and model catalog.
- Capability matching and optional user override.
- Provider-specific request/response normalization.
- Quota/rate-limit detection, bounded retries, and fallback.
- Usage, latency, and failure telemetry.
- Data-handling policy metadata.

The gateway preserves provider-specific errors and limits rather than masking them as generic model failures.

### 4.5 Tool gateway

The only authorized path from agent decisions to external effects. Each tool call has a schema, permission category, project scope, argument validation, approval rule, audit event, timeout, and normalized result.

### 4.6 Execution runner

Owns:
- Workspace creation and cleanup.
- Project checkout and task branch/worktree.
- Command execution.
- Resource and filesystem boundaries.
- Environment-variable injection with redaction.
- Project lifecycle and preview process management.

Project code runs in isolated execution environments, not in the orchestrator process.

### 4.7 Browser and QA runner

Owns:
- Playwright browser lifecycle.
- Configured page journeys and viewport checks.
- Screenshot and trace capture.
- Console, page-error, failed-request, and navigation diagnostics.
- Test result association with task and workspace revision.

### 4.8 GitHub adapter

Owns:
- GitHub App installation discovery.
- Explicit repository selection.
- Permission-scoped installation tokens.
- Repository metadata/tree/content access.
- Authorized branch, commit, pull request, and status/check operations.
- GitHub API rate-limit handling.

The adapter uses only granted installation permissions and selected repositories.

### 4.9 Persistence and artifacts

Relational persistence stores identity, relationships, lifecycle state, settings, and metadata. Artifact storage holds screenshots, traces, test bundles, logs, and generated reports. Large binary artifacts are referenced by metadata rather than placed into relational rows.

## 5. Agent execution sequence

1. Operator submits a task with project, mode, and autonomy settings.
2. API authenticates the operator and resolves project scope.
3. Orchestrator gathers relevant project context and asks the model gateway for analysis/plan.
4. Orchestrator persists the plan, steps, and acceptance criteria.
5. Policy layer checks planned operations and approval requirements.
6. Tool gateway validates each requested operation.
7. Runner or external adapter performs the scoped operation.
8. Tool result and evidence metadata are persisted.
9. Orchestrator evaluates actual results and chooses the next step.
10. Verification and, where permitted, bounded repairs execute.
11. API streams state and evidence summaries to the UI.
12. Final report distinguishes completed, failed, blocked, and untested items.

If approval is required, the task enters Awaiting approval and resumes only after the exact operation is approved.

## 6. Trust boundaries and data flows

### 6.1 Trust zones

1. **Operator zone:** Browser, user input, attachments, and explicit approvals.
2. **Platform control plane:** Web/API, orchestrator, policy, model gateway, and database.
3. **External services:** AI providers, GitHub, public research sites, and hosting providers.
4. **Untrusted execution zone:** Project dependencies, build scripts, generated code, and test processes.
5. **Evidence zone:** Logs, screenshots, traces, test records, and reports with scoped access.

No zone inherits the permissions of another by default.

### 6.2 Prompt/data flow

- User request and selected repository context are assembled for the selected model.
- Secrets are excluded and redacted.
- Repository content, webpages, issues, and tool outputs are labelled as untrusted data.
- Model output is treated as a proposal, not authority.
- Policy validates every tool operation before execution.
- Tool output is captured and returned as evidence.
- Final responses distinguish measured tool results from inference.

### 6.3 Model-provider boundary

Only the model gateway communicates with AI providers. Provider credentials are held server-side or in protected local configuration. The gateway records provider/model selection and handles quotas and errors. Sensitive source code is not sent to a provider unless its configured data terms have been accepted by the owner.

### 6.4 Project execution boundary

The runner mounts only the relevant project workspace and explicitly approved artifacts. It must not mount platform secrets, user SSH directories, browser profiles, platform databases, or the Docker socket into project containers.

## 7. Task state and event architecture

The orchestrator owns task-state transitions. UI clients subscribe to read-only task events. Events are persisted as part of state transitions so refreshes and reconnects do not erase the audit trail.

Core task states:
- Draft
- Planning
- Awaiting approval
- Queued
- Running
- Verifying
- Repairing
- Completed
- Completed with issues
- Blocked
- Failed
- Cancelled

State transitions are explicit and validated. Duplicate tool submissions are prevented using operation/task identifiers and idempotency rules where supported.

## 8. Technology baseline

| Area | Baseline |
|---|---|
| Frontend | Next.js and TypeScript |
| UI components | Tailwind CSS and shadcn/ui |
| Backend | Node.js and TypeScript modular application |
| Data | PostgreSQL |
| Monorepo | pnpm workspace |
| Agent orchestration | Typed custom state machine |
| AI | Provider adapters, including OpenAI-compatible interfaces where suitable |
| GitHub | GitHub App and Git CLI |
| Execution | Docker-based isolated runner |
| Browser QA | Playwright |
| Live updates | Server-Sent Events initially |
| Hosting adapters | Cloudflare Pages/Workers candidate for compatible workloads |

## 9. Proposed repository layout

    sdc-studio/
    ├── apps/
    │   ├── web/
    │   └── runner/
    ├── packages/
    │   ├── agent-core/
    │   ├── ai-gateway/
    │   ├── github-integration/
    │   ├── tool-contracts/
    │   ├── execution/
    │   ├── browser-qa/
    │   └── shared/
    ├── docs/
    ├── tests/
    ├── .github/workflows/
    ├── package.json
    ├── pnpm-workspace.yaml
    ├── tsconfig.base.json
    ├── README.md
    └── PHASE-0-FOUNDATION.md

Do not create all modules until their implementation phase. This document defines intended ownership and boundaries, not a request for premature scaffolding.

## 10. Architecture decision records

| ID | Decision | Reason | Revisit condition |
|---|---|---|---|
| ADR-001 | Modular monolith plus isolated runner | Small initial footprint with clear execution boundary | Measured scaling or isolation need |
| ADR-002 | Local-first initial execution | Avoid mandatory hosted runner cost and keep workspace local | Need for remote collaboration/availability |
| ADR-003 | Provider adapter gateway | Avoid provider lock-in and centralize limits | Material provider capability change |
| ADR-004 | GitHub App with selected repositories | Scoped access and attributable operations | GitHub integration requirement changes |
| ADR-005 | Task branch/worktree, default branch protected | Preserve user work and enable review | Owner explicitly changes repository policy |
| ADR-006 | Playwright for browser evidence | Repeatable browser automation and screenshots | Browser/runtime requirements exceed support |
| ADR-007 | PostgreSQL persistence | Durable related task/project state | Local deployment constraint justifies alternative |

## 11. Architecture change control

A material change to component ownership, trust boundaries, data flow, execution topology, or baseline technology must include the reason, alternatives considered, security/reliability impact, migration effect, and owner approval. Do not introduce microservices, extra databases, or external services without a demonstrated requirement.
