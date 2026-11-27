# SDC Studio — Requirements Traceability Matrix

**Status:** Draft  
**Purpose:** Preserve a stable link between product requirements, roadmap work items, documentation, and later verification.

## 1. How to use this matrix

- Requirement IDs are stable identifiers. Do not casually rename them.
- Product behavior is defined in the Product Specification and User Workflows.
- Architecture and security constraints are defined in their authoritative documents.
- Roadmap item IDs identify implementation work.
- Verification references identify the test/evidence that must exist later.
- A requirement may be accepted only when its implementation and evidence satisfy its acceptance statement.
- When a requirement changes, record the reason and affected roadmap/test references.

## 2. Project and conversation requirements

| ID | Requirement | Roadmap | Verification expected |
|---|---|---|---|
| FR-PROJ-01 | Create a project without a remote repository | P3-05 | Create/list/open project integration test |
| FR-PROJ-02 | Connect an explicitly selected repository | P5-01 to P5-04 | GitHub installation and repository-scope test |
| FR-PROJ-03 | Isolate project workspaces and histories | P3-05, P6-01 | Cross-project access and workspace isolation tests |
| FR-PROJ-04 | Display repository, branch, and baseline state | P5-05, P5-06 | Repository metadata and commit SHA verification |
| FR-CHAT-01 | Create and retain conversations | P3-06 | Persistence and reload test |
| FR-CHAT-02 | Stream responses when provider supports streaming | P3-07, P4-01 | Streaming adapter and UI test |
| FR-CHAT-03 | Accept supported file/image attachments with validation | P3-08 | MIME, size, malformed file and access-control tests |

## 3. Modes and autonomy

| ID | Requirement | Roadmap | Verification expected |
|---|---|---|---|
| FR-MODE-01 | Provide the eight approved operating modes | P3-09, P4-10, P4-11 | Mode selection and orchestrator policy tests |
| FR-MODE-02 | Keep mode independent of model choice | P3-09, P4-07 | UI state and API contract tests |
| FR-AUTO-01 | Provide Ask, Guided, Auto execution levels | P3-09, P4-09, P4-12 | Autonomy policy matrix tests |
| FR-AUTO-02 | Apply tool permissions and approval gates server-side | P4-12, P5-09, P6-03 | Unauthorized operation and policy enforcement tests |
| FR-AUTO-03 | Avoid repeated prompts for routine authorized actions | P4-12, P6-08 | Workflow tests for pre-authorized reversible operations |
| FR-AUTO-04 | Require approval for defined high-impact actions | P4-12, P8-05, P8-08 | Approval required/rejected/expired tests |

## 4. AI and orchestration

| ID | Requirement | Roadmap | Verification expected |
|---|---|---|---|
| FR-AI-01 | Configurable provider/model catalog | P4-01, P4-04 | Model registry CRUD and capability validation |
| FR-AI-02 | User model choice and optional task-based routing | P4-07 | Model override and router selection tests |
| FR-AI-03 | Record provider/model, latency, usage and failures | P4-06 | Telemetry persistence and redaction tests |
| FR-AI-04 | Respect provider rate limits and data terms | P4-05, P4-06, P4-08 | 429/backoff, disabled provider and data-policy tests |
| FR-AI-05 | Handle unavailable models, quota errors, timeouts and fallback | P4-08 | Simulated provider error/fallback tests |
| FR-AI-06 | Support optional local inference | P4-03 | Local adapter contract test |
| FR-AGENT-01 | Create plans, steps and acceptance criteria | P4-10 | Planning fixture and persisted plan test |
| FR-AGENT-02 | Assign appropriate logical specialist roles | P4-11 | Role routing and specialist output contract tests |
| FR-AGENT-03 | Persist task and step states | P4-09 | State transition and restart/resume tests |
| FR-AGENT-04 | Route all mutations/commands through tool gateway | P4-12 | Architecture boundary and authorization tests |
| FR-AGENT-05 | Record every tool invocation and evidence | P4-12 | Audit completeness tests |
| FR-AGENT-06 | Stop/request approval when permission exceeded | P4-12 | Denied tool and approval workflow tests |

## 5. Repository and execution

| ID | Requirement | Roadmap | Verification expected |
|---|---|---|---|
| FR-REPO-01 | Inspect repository before changes | P5-05 | Read-only discovery test; no writes during connect |
| FR-REPO-02 | Preserve baseline and user changes | P5-06, P5-08 | Dirty worktree and baseline recovery tests |
| FR-REPO-03 | Use task branches/worktrees; no force-push by default | P5-08, P5-09 | Branch target and Git command policy tests |
| FR-REPO-04 | Display changed files and diffs | P5-10 | Diff correctness test |
| FR-REPO-05 | Create commits and PRs on authorized branches | P5-11 | Commit/PR integration test |
| FR-EXEC-01 | Execute commands outside platform control plane | P6-01, P6-02 | Container isolation test |
| FR-EXEC-02 | Record command, cwd, time, exit code and redacted output | P6-04 | Command audit and redaction test |
| FR-EXEC-03 | Enforce workspace/resource/network limits | P6-02, P6-09 | Path, CPU, memory, timeout and egress tests |

## 6. QA, research and operations

| ID | Requirement | Roadmap | Verification expected |
|---|---|---|---|
| FR-QA-01 | Start detected/configured development server | P7-01 | Readiness and process lifecycle tests |
| FR-QA-02 | Show scoped embedded preview | P7-02 | Preview access and origin isolation tests |
| FR-QA-03 | Run applicable project tests/checks | P7-07 | Test discovery and result recording |
| FR-QA-04 | Capture screenshots and browser diagnostics | P7-03 to P7-06 | Screenshot, console, page-error and network fixtures |
| FR-QA-05 | Link evidence to task/test run | P7-11 | Artifact ownership/link integrity test |
| FR-QA-06 | Bounded diagnose-fix-retest loop | P7-09, P7-10 | Attempt/time budget and regression test |
| FR-QA-07 | Report unresolved and untested areas | P7-11 | Report correctness test |
| FR-RES-01 | Research public documentation and cite sources | P4-11, P4-12 | Source metadata and citation presence test |
| FR-REPORT-01 | Produce evidence-backed task report | P4-12, P7-11 | Report schema and evidence traceability test |
| FR-DEPLOY-01 | Recommend hosting based on architecture and runtime | P8-01, P8-02 | Hosting capability rule test |
| FR-DEPLOY-02 | Require authorization before production deployment | P8-05, P8-08 | Production approval gate test |
| FR-MAINT-01 | Support authorized maintenance and dependency improvements | P8-10 | Maintenance task and regression test |

## 7. Non-functional traceability

| ID | Quality attribute | Primary document | Verification |
|---|---|---|---|
| NFR-SEC-01 | Secrets not exposed to browser or model | Security Specification | Secret scan, API response and prompt redaction tests |
| NFR-SEC-02 | Untrusted code isolated from control plane | Architecture / Security | Container boundary tests |
| NFR-TRACE-01 | Actions and results auditable | Data Model / Contracts | Event completeness and correlation tests |
| NFR-REC-01 | Changes attributable and recoverable | Workflows / Security | Baseline, diff and revert tests |
| NFR-REL-01 | Failure states explicit | Contracts / Workflows | Provider/tool failure tests |
| NFR-REL-02 | Retries bounded and quota-aware | AI Provider Strategy | Retry budget and rate-limit tests |
| NFR-UX-01 | Current status and next action visible | Workflows | Task UI acceptance tests |
| NFR-EXT-01 | Provider/hosting replaceability | Architecture / Contracts | Adapter contract tests |
| NFR-COST-01 | Usage and free-tier constraints visible | Provider Strategy | Usage display and quota state tests |
| NFR-EVID-01 | Test/deployment claims evidence-backed | QA Plan | Report-to-artifact traceability test |

## 8. User freedom acceptance

| ID | Criterion | Verification |
|---|---|---|
| FREEDOM-01 | No arbitrary SDC-specific topic bans for legitimate authorized engineering | Review policy configuration and task acceptance tests |
| FREEDOM-02 | Owner can select configured provider/model and optional local model | Provider settings and routing tests |
| FREEDOM-03 | Owner can configure project/tool permissions and autonomy | Settings and policy tests |
| FREEDOM-04 | Routine authorized actions avoid unnecessary repeated prompts | Guided/Auto workflow tests |
| FREEDOM-05 | External restrictions are attributed accurately | Provider quota/permission failure tests |
| FREEDOM-06 | The system never claims literal zero restrictions across external services | Product wording and error/report review |

The 10/10 freedom score is a product target for legitimate, authorized work. It is not a factual claim that external providers or platforms have no restrictions.

## 9. Change control

When a requirement changes:
1. Preserve the existing ID if its intent remains the same.
2. Add a new ID if it is a genuinely new requirement.
3. Update the product spec, roadmap mapping, workflow, and verification plan.
4. Record impact on already approved decisions and phases.
5. Obtain owner approval before adding scope.
