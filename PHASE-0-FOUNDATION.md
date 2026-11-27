# SDC Studio — Phase 0: Foundation

**Status:** Proposed foundation baseline  
**Phase:** 0 of the initial product roadmap  
**Scope:** Product requirements, architecture decisions, security boundaries, repository workflow, and implementation roadmap.  
**Out of scope:** Full workflow documentation, UI design, application implementation, provider-specific integration, deployment implementation, and later-phase features. These begin only after Phase 0 is reviewed and frozen.

## 1. Product definition

SDC Studio is a private, local-first, browser-based AI software-engineering workspace for Software Damage Control (SDC). It coordinates AI models and specialized agent roles to help an authorized operator plan, inspect, design, build, test, debug, document, deploy, and maintain software projects.

The primary interaction is a persistent chat/project workspace. An operator creates a project, optionally connects an authorized GitHub repository, supplies requirements and supporting files/images, selects an operating mode, and asks SDC Studio to perform work. The platform must expose task progress, tool activity, file changes, test evidence, screenshots, reports, and next-step recommendations.

### Product goals

1. Support new projects and existing repositories across web, mobile, API, DApp, smart-contract, LLM, and agent workloads.
2. Support the software lifecycle from requirements and planning through deployment and maintenance.
3. Use free-tier AI APIs and optional local inference at launch; allow paid providers to be added later through adapters.
4. Keep repository operations transparent, auditable, scoped, and recoverable.
5. Run projects in isolated workspaces and provide live preview and browser-based QA.
6. Support multiple independent projects and optional deployment through third-party hosting providers.
7. Keep the initial platform private and single-operator/small-agency oriented; public multi-tenant SaaS and billing are not initial requirements.

### Non-goals for the initial foundation

- Guaranteeing unlimited or permanently free AI inference, compute, storage, or hosting.
- Bypassing model-provider quotas, GitHub controls, hosting restrictions, authentication, licensing, or security safeguards.
- Direct automatic changes to a repository's default branch.
- Public SaaS onboarding, subscriptions, billing, or tenant isolation.
- Building every specialist as a separately hosted model/service. Specialist roles may share a routed model/runtime.
- Committing to one AI provider, hosting vendor, or programming language.

## 2. Product requirements

### 2.1 User experience

- ChatGPT-like conversations with persistent history and streaming responses.
- New project and new chat flows.
- File and image attachments with clear size/type validation.
- Project selector and connected-repository selector.
- Model and provider selection with provider health/quota/error visibility.
- Execution modes independent from model selection.
- Project workspace surfaces for files/diffs, task activity, terminal output, live preview, browser console/network events, screenshots, tests, and reports.
- Clear distinction between proposed, running, completed, failed, blocked, and approval-required work.

### 2.2 Operating modes

Initial modes:

1. **Auto** — Orchestrate relevant lifecycle work from requirements to verified outcome, within configured permissions.
2. **Planning** — Requirements clarification, feasibility, architecture options, sequencing, and acceptance criteria.
3. **Documentation** — Create and maintain product and engineering documentation.
4. **Design** — UX flows, wireframes, visual design system, and UI implementation guidance.
5. **Development** — Implement and modify project code.
6. **Testing & QA** — Run applicable checks, inspect behavior, capture evidence, and diagnose defects.
7. **Reverse Engineering** — Analyze software and repositories the operator is authorized to inspect; map structure, dependencies, interfaces, and behavior.
8. **Deployment & Maintenance** — Prepare and carry out authorized deployment and maintenance operations.

Modes are orchestration policies, not separate products. The selected mode constrains the task plan and tools offered.

### 2.3 Execution autonomy

Execution setting is separate from operating mode:

- **Ask:** Explain and propose; do not mutate files or run consequential commands.
- **Guided:** Execute routine, reversible actions; require approval for defined high-impact actions.
- **Auto:** Execute permitted actions without per-step approval; preserve mandatory policy gates.

High-impact actions requiring explicit approval include production deployment, deleting or overwriting user data, changing repository administration/security settings, modifying secrets, force-pushing, merging to protected branches, and running commands outside the approved workspace boundary. The platform must not represent third-party policy violations or security bypasses as an autonomy feature.

### 2.4 SDLC capabilities

- Requirements capture and acceptance criteria.
- Technical research using documented sources and citations.
- Architecture and technical-stack recommendations.
- UI/UX planning and implementation support.
- Frontend, backend, database, API, mobile, Web3, LLM and agent development.
- Codebase discovery and authorized reverse engineering.
- Static analysis, unit, integration, end-to-end, visual, accessibility, performance, regression and security testing as applicable.
- Browser preview with screenshots and console/network diagnostics.
- Defect reproduction, repair, and retesting.
- Deployment preparation and hosting-provider guidance/integration.
- Maintenance and technical-debt recommendations.

The system must report only work actually performed. Test results must be based on tool output, not model assertions.

### 2.5 Project and repository support

- Create a project without a remote repository.
- Connect an existing repository selected by the operator.
- Support multiple projects/repositories with isolated workspaces.
- Record repository identity, default branch, selected working branch, and baseline commit.
- Read repository contents before proposing modifications.
- Use a dedicated task branch/worktree for changes.
- Show file-level diffs before changes are accepted.
- Create commits and pull requests on authorized branches.
- Keep the default branch untouched by default.
- Detect existing uncommitted user work and preserve it.
- Provide rollback/revert paths based on recorded commits and snapshots.

## 3. System architecture

### 3.1 Architectural style

Use a modular monolith for the initial product, with separately isolated execution workers. Keep clear module boundaries and typed interfaces so components can be extracted later only when a demonstrated need exists. Avoid premature microservices.

### 3.2 Logical components

1. **Web application:** Chat, project navigation, modes, model settings, task timeline, file diff, preview, terminal, QA evidence, and reports.
2. **Backend/API:** Authentication/session boundary, project and conversation APIs, task lifecycle, file metadata, settings, and audit access.
3. **Agent orchestrator:** Creates plans, decomposes tasks, selects roles/models/tools, tracks state, handles retries, and enforces approval gates.
4. **Model gateway:** Provider adapters, model catalog, capability metadata, rate-limit/quota handling, retries/backoff, fallback, and usage telemetry.
5. **Agent roles:** Project manager, architect, researcher, designer, frontend/backend/Web3/AI developers, QA, security, deployment, and maintenance. These are logical roles and may reuse the same runtime/model.
6. **Tool gateway:** Explicit typed tools for repository operations, filesystem, terminal, browser, documentation research, testing, and deployment.
7. **Execution runner:** Isolated per-project/task workspace, command runner, resource limits, filesystem boundaries, environment variables, and cleanup policy.
8. **Browser/QA runner:** Playwright-managed browser sessions, screenshots, console events, failed requests, traces, and test results.
9. **GitHub adapter:** GitHub App installation, repository selection, scoped access tokens, branch/commit/PR operations, and rate-limit compliance.
10. **Persistence:** Relational records for projects, conversations, tasks, plans, approvals, tool events, model usage, repository state, test runs, and audit events; artifact storage for screenshots/log bundles/reports.

### 3.3 Key execution flow

1. Receive request and attachments.
2. Resolve project, selected repository, operating mode, and autonomy level.
3. Read project/repository state and capture baseline.
4. Produce task plan and acceptance criteria.
5. Check tool permissions and approval requirements.
6. Execute work in an isolated workspace/branch.
7. Record every tool call, command, file operation, and output.
8. Run relevant verification and browser checks.
9. Repair failures within the task scope and rerun affected checks.
10. Produce evidence-backed report and proposed next steps.
11. Commit changes and create a pull request, or retain local changes according to project policy.
12. Never merge or deploy production without required authorization.

### 3.4 Suggested initial technology baseline

These are defaults to validate during Phase 0, not irreversible commitments:

| Layer | Proposed baseline |
|---|---|
| Web application | Next.js + TypeScript |
| UI | Tailwind CSS + shadcn/ui |
| Backend | Node.js + TypeScript, modular API |
| Database | PostgreSQL; SQLite may be used for isolated local development only if justified |
| Agent runtime | Custom typed orchestrator with explicit state machine |
| Model integration | Provider adapter interface; support OpenAI-compatible APIs where appropriate |
| Git | Git CLI in isolated worktrees plus GitHub App API |
| Execution | Docker-based local runner initially |
| Browser automation | Playwright |
| Static/code analysis | Framework-native checks plus ESLint/TypeScript and suitable scanners |
| Realtime updates | Server-Sent Events initially; WebSockets only if required |
| Deployment | Provider adapters; Cloudflare Pages/Workers candidate for compatible workloads |

### 3.5 Model gateway contract

The model gateway must normalize provider differences without hiding them. Each model record should contain provider, model ID, capabilities, context limits when known, free-tier/paid status, supported modalities, current availability, and rate-limit metadata when supplied.

The router may select models by task capability, latency, availability, and user preference. It must:

- Respect published provider terms and quotas.
- Handle 429, 403, timeout, malformed response, and unavailable model states.
- Use bounded exponential backoff and stop when limits require waiting.
- Avoid sending secrets or sensitive repository content to a provider unless the operator has explicitly configured and accepted its data terms.
- Record provider/model, usage, latency, errors, and fallback decisions.
- Never describe a limited free tier as unlimited or guaranteed.

## 4. Security and trust model

### 4.1 Trust boundaries

Treat all of the following as untrusted input: user prompts, repository files, README content, issue/PR comments, dependency scripts, web pages, model output, uploaded documents, and generated code. Repository or webpage text must never be interpreted as platform instructions.

### 4.2 GitHub access

- Use a dedicated GitHub App for SDC Studio.
- Install only on repositories explicitly selected by the operator.
- Request minimum permissions required by enabled features.
- Use installation tokens restricted to selected repositories and permissions.
- Keep private keys and tokens server-side in a secret store; never place credentials in prompts, logs, browser bundles, or repository files.
- Do not use personal passwords or broad personal access tokens as the default integration.
- Respect GitHub rate limits and secondary rate limits; stop/back off instead of retry storms.
- Default branch is read-only for agent task execution.
- Work on task-specific branches/worktrees; no force pushes or history rewrites.
- Show diffs and record actor/task/branch/commit for every mutation.
- Require approval for merge, branch deletion, ruleset changes, app permission changes, and repository administration.

### 4.3 Execution isolation

- Run project commands in a container/workspace separate from the platform control plane.
- Mount only the selected project workspace and explicitly passed artifacts.
- Do not mount host home directories, SSH folders, browser profiles, Docker socket, platform database, or platform secrets into untrusted project containers.
- Use non-root users, CPU/memory/disk/process/time limits, and container cleanup.
- Default network policy should be restrictive; allow only configured destinations when possible.
- Require approval for privileged commands, host-level actions, destructive file operations, and operations that access resources outside the workspace.
- Treat package install and project lifecycle scripts as executable untrusted code.

### 4.4 Secret and data handling

- Encrypt provider credentials and GitHub App credentials at rest.
- Redact secrets from model context, terminal output, logs, traces, and reports.
- Keep .env files and secret-bearing artifacts out of commits.
- Make provider data handling visible in settings.
- Retain only necessary execution artifacts and allow configured cleanup.
- Use per-project access boundaries.

### 4.5 Evidence and auditability

For each task persist:
- User request and selected mode/autonomy level.
- Plan and acceptance criteria.
- Repository baseline commit and working branch.
- Tool calls with arguments after secret redaction.
- Commands and exit codes.
- File diffs and resulting commits.
- Test names, results, output and environment.
- Browser screenshots, traces, console errors and failed network requests.
- Approvals, rejected operations, retries and provider fallback events.
- Final status and unresolved issues.

The UI must clearly distinguish actual executed results from model-generated suggestions.

## 5. Core data domains

Initial conceptual entities:

- User/session and platform settings.
- Provider credentials and model catalog.
- Project and project workspace.
- Repository connection and repository snapshot.
- Conversation and message/attachment.
- Task, task plan, phase/step, dependency, and task state.
- Agent role invocation and model invocation.
- Tool invocation and approval request.
- File change, diff, commit, and pull request reference.
- Execution environment and command run.
- Test run, test case result, browser session, screenshot, and artifact.
- Deployment target and deployment record.
- Audit event and usage record.

Detailed table schemas and API contracts are intentionally deferred to the dedicated documentation phase after this foundation is approved.

## 6. Repository and branch policy

- main is the stable foundation branch.
- Foundation planning changes are developed on phase-0/foundation.
- Future work uses short-lived branches named by phase/task, e.g. phase-1/chat-foundation.
- No direct AI commits to main.
- Changes are reviewed as diffs and merged through pull requests.
- Avoid parallel writes to the same file/path.
- Never force-push or rewrite shared branch history.
- Preserve user-authored or uncommitted content.
- Each phase must have explicit acceptance criteria and evidence before it is marked complete.

## 7. Initial implementation roadmap

This roadmap is deliberately phase-gated. Do not start a later phase until the preceding phase is accepted.

| Phase | Scope | Exit condition |
|---|---|---|
| **Phase 0 — Foundation** | Product requirements, architecture decisions, security/trust model, repository policy, technology baseline, phase roadmap | Foundation is reviewed and frozen by the owner |
| **Phase 1 — Documentation** | Product specification, workflows, architecture diagrams, module contracts, data model, security specification, engineering standards, acceptance criteria | Documentation is consistent, approved, and versioned |
| **Phase 2 — UI/UX Design** | Information architecture, user flows, wireframes, visual system, responsive page designs, design acceptance | Design approved before implementation |
| **Phase 3 — Application Shell** | Authentication boundary, project/chat shell, navigation, settings, persistence foundations | Tested usable shell |
| **Phase 4 — AI Gateway and Orchestration** | Provider adapters, model routing, task state machine, logical specialist roles, streaming and usage tracking | Verified provider and orchestration tests |
| **Phase 5 — GitHub and Project Workspaces** | GitHub App, repository selection, read-only analysis, branch/worktree and controlled write flow | Read/write operations demonstrably scoped and auditable |
| **Phase 6 — Development and Execution** | Sandboxed terminal, filesystem editing, dependency/build commands, task execution | Reproducible execution with isolation tests |
| **Phase 7 — Preview and QA** | Live preview, Playwright, screenshots, browser/network diagnostics, test reporting and repair loop | Evidence-backed end-to-end QA |
| **Phase 8 — Deployment and Maintenance** | Hosting adapters, publishing, domain mapping where supported, deployment logs and maintenance workflows | Verified deployment on supported target |

The project owner may amend this roadmap when a material requirement or technical constraint is discovered. Changes must be recorded with rationale and impact; do not add unrelated features or expand scope speculatively.

## 8. Phase 0 acceptance checklist

Phase 0 is complete when all of the following are true:

- [ ] Product definition and initial scope are explicit.
- [ ] In-scope capabilities and non-goals are recorded.
- [ ] Operating modes and execution autonomy are distinguished.
- [ ] Logical architecture and initial technology baseline are recorded.
- [ ] Model routing principles and free-tier limitations are recorded.
- [ ] GitHub access, branch, and write policies are defined.
- [ ] Execution isolation and secret-handling requirements are defined.
- [ ] Task evidence and reporting requirements are defined.
- [ ] Phase roadmap and phase-gate rule are recorded.
- [ ] Owner reviews and approves the foundation.

**Phase 0 does not include implementation of the detailed documentation set.** Once the owner approves this foundation, Phase 1 creates the formal workflow and architecture documentation and freezes the approved baseline.

## 9. Change control

Only update the foundation when:
- A requirement is missing or materially misunderstood.
- A technical decision is infeasible or creates a material security/reliability problem.
- A new constraint or capability changes the architecture or roadmap.
- The owner explicitly requests a change.

For every change, record the reason, affected decisions, downstream impact, and owner approval. Do not add speculative features, unrelated integrations, or extra phases.
