# SDC Studio — Acceptance Criteria

**Status:** Draft  
**Purpose:** Define verifiable completion criteria for Phase 1 documentation and preserve traceability for later product phases.

## 1. Acceptance principles

- A requirement is accepted only when expected behavior and verification approach are explicit.
- A tool result or test report must provide evidence; model-generated claims alone are insufficient.
- Not run, not configured, and blocked are not passed.
- The owner approves phase completion.
- A phase cannot silently expand into later-phase work.

## 2. Phase 1 documentation exit criteria

Phase 1 is complete when:

- [ ] Product specification formalizes approved goals, scope, modes, autonomy, and functional requirements.
- [ ] User workflows cover blank project creation, GitHub connection, repository task execution, planning, documentation, design handoff, preview/QA, research, deployment, and reporting.
- [ ] Architecture documentation includes component responsibilities, logical diagrams, execution flow, trust boundaries, topology, and architecture decisions.
- [ ] Module contracts define responsibilities, allowed dependencies, inputs/outputs, errors, events, and idempotency expectations.
- [ ] Data model defines core entities, relationships, statuses, integrity, retention, and secret references.
- [ ] Security specification defines trust boundaries, GitHub permissions, provider data handling, prompt-injection boundaries, execution isolation, approvals, audit, and incident response.
- [ ] Engineering standards define code quality, repository policy, testing, secrets, observability, documentation, and phase discipline.
- [ ] Requirement and acceptance identifiers are stable enough to reference in implementation tasks.
- [ ] Cross-document terminology and architecture are consistent.
- [ ] No UI or application implementation has been introduced.
- [ ] Owner reviews and approves the complete documentation set.

## 3. Cross-document consistency checks

- Product modes match across product specification and workflows.
- Ask, Guided, and Auto are autonomy settings and are not confused with operating modes.
- Model/provider selection is independent of task mode.
- Agent roles use the orchestrator and tool gateway; they do not bypass policy.
- Project code executes in the isolated runner, not in the platform control plane.
- GitHub access is limited to selected repositories and granted permissions.
- Default branch protection and task-branch workflow are consistent throughout.
- Tool activity and task reports refer to recorded evidence.
- User-freedom target is 10/10 for legitimate authorized work, while external constraints are clearly disclosed.
- Deployment and production-impacting operations retain explicit approval requirements.

## 4. Requirement-to-document traceability

| Requirement area | Authoritative document | Later verification |
|---|---|---|
| Product scope and goals | Product specification | Product scope review and feature acceptance |
| Chat, project and attachments | Product specification; User workflows | UI and API functional tests |
| Operating modes and autonomy | Product specification; User workflows | Mode/task policy tests |
| Provider selection and routing | Product specification; Architecture; Module contracts | Provider adapter and routing tests |
| Repository access and branch safety | User workflows; Architecture; Security | GitHub integration and mutation tests |
| Execution isolation | Architecture; Security | Container/isolation tests |
| Task tools and evidence | Workflows; Contracts; Data model | Audit and evidence tests |
| Preview and browser QA | Workflows; Architecture; Contracts | Playwright and screenshot checks |
| Deployment and maintenance | Workflows; Product specification | Deployment adapter acceptance |
| Engineering quality | Engineering standards | CI and PR checks |

## 5. Later product-level acceptance outline

These are targets for later implementation phases, not work authorized during Phase 1.

### 5.1 Project and repository

- A blank project can be created without a remote repository.
- An existing repository can be selected only from the authorized GitHub App installation.
- Initial connection performs read-only discovery and records a baseline commit.
- Task changes occur on an isolated task branch/worktree.
- The default branch is not modified by ordinary agent task execution.
- Diffs, commits, and pull request references are visible and auditable.

### 5.2 AI and orchestration

- At least one configured provider can complete a model request through the normalized gateway.
- Provider selection and model capabilities are visible.
- Quota, rate-limit, unavailable model, and timeout conditions are represented accurately.
- Agent plans and task state transitions are persisted.
- Tool calls pass through policy validation and produce audit events.
- Provider failure does not result in fabricated task completion.

### 5.3 Execution and debugging

- A project command runs only in the isolated workspace.
- Command outputs and exit codes are recorded.
- Resource and timeout limits are enforced.
- File changes are diffable and recoverable.
- An execution failure can be diagnosed without exposing platform secrets.

### 5.4 Preview and QA

- A supported project can start and expose a scoped preview.
- Browser tests execute only defined journeys and viewports.
- Screenshots, console errors, failed requests, and traces are linked to the test run.
- A repair is followed by relevant retesting.
- Reports distinguish passed, failed, skipped, blocked, and not-run tests.

### 5.5 Deployment

- Hosting compatibility and plan limitations are assessed before deployment.
- Required production secrets are configured through protected settings.
- Production deployment requires approval.
- Deployment ID, URL, logs, and verification outcome are recorded.
- The system does not promise that a free hosting plan supports every application architecture.

### 5.6 User freedom and transparency

- Owner can configure available providers/models and optional local models.
- Broad legitimate engineering tasks are not blocked by arbitrary SDC-specific topic restrictions.
- Routine authorized operations are not burdened with unnecessary repeated confirmations.
- High-impact actions retain explicit approval.
- External provider/platform/legal/security constraints are clearly attributed.
- A task report distinguishes actually executed operations and tests from suggestions.

## 6. Phase acceptance record

When Phase 1 is approved, record:
- Approver and approval date.
- Documentation branch and PR.
- Merge commit SHA.
- Accepted exceptions or deferred clarifications.
- Confirmation that Phase 2 may begin.

No subsequent phase begins before this acceptance record exists.
