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


## 7. Complete project-documentation acceptance

The Phase 1 documentation set is not considered complete based only on the original eight specification documents. The following project-control and continuity documents are also required:

- [ ] Master Implementation Roadmap lists the approved Phase 0–8 sequence, work item IDs, dependencies, deliverables, and exit criteria.
- [ ] Project Status Register identifies the current phase, completed work, active work, blocked items, pending owner decisions, and one immediate next action.
- [ ] Requirements Traceability Matrix maps every functional and non-functional requirement in the Product Specification to roadmap work and future verification evidence.
- [ ] Decision Log distinguishes accepted, provisional, pending, rejected, and superseded decisions and records the rationale and phase where pending decisions will be resolved.
- [ ] Agent and Tool Catalog defines specialist responsibilities, handoff expectations, tool families, permission categories, and boundaries.
- [ ] Testing and QA Strategy defines applicable test layers, evidence capture, browser diagnostics, bounded repair/retest, and reporting rules.
- [ ] Integration Strategy defines provider-independent boundaries for AI providers, GitHub, research, and hosting; vendor-specific free quotas and terms remain subject to verification when integrated.
- [ ] Project Memory and Change-Control Guide defines the source-of-truth hierarchy, resume procedure, status-update procedure, phase gates, and change-control process.
- [ ] Documentation Index links every authoritative document and explains where to find status, roadmap, requirements, decisions, and continuity guidance.
- [ ] Root README points contributors to the foundation, documentation index, status register, and roadmap.

## 8. Documentation consistency and project-memory checks

Before Phase 1 owner approval, verify:

- All 16 numbered documents and the documentation index exist in the Phase 1 branch.
- Internal relative links resolve to actual files.
- Requirement identifiers are unique and every Product Specification FR/NFR identifier appears in the Traceability Matrix.
- Roadmap phase names and order match the Phase 0 Foundation.
- Project Status phase states and next action match actual GitHub branch/PR state.
- The Decision Log does not label provisional decisions as owner-approved.
- Agent roles do not bypass the orchestrator, tool gateway, or security policy.
- Testing evidence requirements match the User Workflows and Security Specification.
- Provider and hosting options are labelled as candidates until current terms and capabilities are verified during their implementation phases.
- The 10/10 user-freedom target is consistently scoped to legitimate, authorized work, with external constraints disclosed.
- Phase 1 remains documentation-only; no UI/UX design, source-code implementation, live provider integration, runner, preview, or deployment work is included.
- All unresolved assumptions and deferred technical decisions are listed with the phase in which they must be verified.

## 9. Documentation completeness boundary

Phase 1 produces a sufficiently detailed planning, requirements, architecture, workflow, security, and project-control baseline for the next phase. It does not pretend that implementation-specific details already exist.

The following details must be finalized when evidence becomes available in their assigned phases:
- Exact REST routes, request/response schemas, and generated API references: during implementation of the relevant backend modules.
- Exact SQL column types, indexes, constraints, and migrations: during Phase 3 database implementation.
- Exact cloud model IDs, free-tier quotas, data terms, and model-quality measurements: during Phase 4 provider evaluation.
- Exact GitHub App events, permission list, callbacks, and webhook contract: during Phase 5 implementation.
- Exact container images, runtime profiles, mounts, command policy, and network rules: during Phase 6 isolation implementation.
- Exact preview proxy, browser profile, supported engines, and screenshot workflow: during Phase 7.
- Exact hosting plan, deployment API, domain/DNS, and rollback details: during Phase 8.

These are intentionally deferred rather than invented. When finalized, add or update the relevant implementation documentation, record the decision, update the traceability matrix, and keep the roadmap/status synchronized.

## 10. Phase 1 acceptance record

When Phase 1 is approved, record:
- Approver and approval date.
- Documentation branch and PR.
- Merge commit SHA.
- Documentation version or baseline reference.
- Accepted exceptions or deferred clarifications.
- Confirmation that Phase 2 may begin.


## 11. Phase 1 approval and merge record

- **Phase:** 1 — Detailed Documentation
- **Owner approval:** Received 2026-11-27.
- **Pull request:** #2 — Phase 1: Complete SDC Studio project documentation
- **Merged:** 2026-11-27
- **Merge method:** Squash
- **Merge commit:** b3ebadd4065400b4155c15469b016520a692d987
- **Accepted deliverables:** 16 numbered documentation files, documentation index, updated root README, and approved Phase 0 foundation retained on main.
- **Deferred implementation details:** Exact API routes, final SQL migrations, live provider/model configuration, GitHub App registration and permissions, runner profiles, preview routing, and hosting-provider-specific deployment behavior remain assigned to their implementation phases.
- **Phase 2 authorization:** UI/UX Design may begin. Application implementation remains out of scope until Phase 2 is accepted.
