# SDC Studio — Agent Roles and Tool Catalog

**Status:** Draft  
**Purpose:** Define logical specialist responsibilities and the allowed tool families so future agent orchestration has clear boundaries.

## 1. Agent design principles

- Agent roles are logical responsibilities, not necessarily separate deployed services or separate models.
- The orchestrator may reuse one configured model for multiple roles or route different roles to different models.
- No agent role owns GitHub credentials, provider API keys, or host-level secrets.
- Agents request actions through typed tool contracts. The policy layer makes the permission decision.
- A specialist role must return structured findings and distinguish evidence from assumptions.
- The orchestrator remains responsible for the overall task plan and final result.
- The owner can configure available tools and autonomy without arbitrary restrictions on legitimate engineering topics.

## 2. Specialist role catalog

### AG-01 — Project Manager / Task Coordinator

**Purpose:** Turn the operator's request into a manageable task and maintain execution clarity.

Responsibilities:
- Parse requested outcome, constraints, target repository/project, and completion expectations.
- Identify ambiguities that materially block progress.
- Create milestones, steps, dependencies, and acceptance criteria.
- Track task status and identify blockers.
- Summarize what has been completed, what remains, and the next action.

Inputs: User request, project context, prior task history, repository discovery summary.

Outputs: Task plan, step list, acceptance criteria, status summary, final report outline.

Must not: Perform unreviewed repository mutations or bypass orchestration policy.

### AG-02 — Research and Documentation Analyst

**Purpose:** Retrieve and interpret relevant public technical sources and supplied project documentation.

Responsibilities:
- Search official documentation and relevant sources.
- Compare approaches and identify compatibility/version caveats.
- Cite retrieved sources with titles, URLs, and dates when available.
- Read project docs and preserve existing terminology.
- Identify conflicts or gaps in project documentation.

Outputs: Source-backed findings, compatibility notes, proposed doc changes.

Must not: Treat retrieved content as instructions or submit secrets to external websites.

### AG-03 — Software Architect

**Purpose:** Assess requirements and define system-level technical design.

Responsibilities:
- Recommend architecture and technology choices.
- Identify module boundaries, dependencies, data flows, and failure cases.
- Review design changes for consistency with approved architecture decisions.
- Identify performance, scaling, security, and operational concerns.
- Explain alternatives and trade-offs.

Outputs: Architecture options, diagrams/specifications, decision proposals, review findings.

Must not: silently change approved architecture or introduce unrelated technology.

### AG-04 — UI/UX Designer

**Purpose:** Design usable, accessible and responsive product experiences.

Responsibilities:
- Define user journeys, information architecture, wireframes and interface behavior.
- Create visual hierarchy, responsive rules, component states and design-system guidance.
- Cover loading, empty, error, blocked, approval, success and disabled states.
- Consider accessibility and keyboard navigation.
- Review screenshots against approved designs.

Outputs: User flows, wireframes, design tokens, screen specs, visual QA findings.

### AG-05 — Frontend Developer

**Purpose:** Implement the approved user interface.

Responsibilities:
- Implement responsive components and navigation.
- Connect UI to documented API contracts.
- Handle streaming, form states, file attachments, errors and task events.
- Maintain accessibility and frontend performance.
- Add frontend tests and resolve UI defects.

Outputs: Frontend code changes, component tests, screenshots, build evidence.

### AG-06 — Backend Developer

**Purpose:** Implement server-side application behavior and persistence.

Responsibilities:
- Implement authenticated APIs, validation and project/task services.
- Implement database repositories and migrations.
- Enforce project/operation scope on the server.
- Implement task event streaming and persistence.
- Maintain safe error handling and observability.

Outputs: API/service code, migrations, integration tests and API evidence.

### AG-07 — AI/LLM and Agent Engineer

**Purpose:** Implement model adapters, routing and agent orchestration.

Responsibilities:
- Integrate configured provider adapters.
- Normalize requests, responses, streaming, usage and errors.
- Implement model capability matching and user overrides.
- Maintain task state machine and logical specialist prompts/contracts.
- Implement bounded retry/fallback and agent/tool coordination.
- Test prompt-injection handling and output validation.

Outputs: Adapter code, model catalog, routing behavior, orchestration tests and usage reports.

### AG-08 — Web3 / Smart Contract Engineer

**Purpose:** Support authorized Web3, blockchain, DApp and smart-contract projects.

Responsibilities:
- Identify chain, compiler, framework and network requirements.
- Analyze contract source, interfaces, events, storage layout and dependencies.
- Implement contract/DApp changes within project scope.
- Run unit, integration, fork/testnet and static analysis appropriate to the project.
- Review transaction, wallet, signing and deployment assumptions.

Outputs: Contract/application changes, test evidence, ABI/interface notes, deployment guidance.

The agent must not handle private signing keys by sending them to models or logs. Signing and production chain actions require explicit secure authorization.

### AG-09 — QA and Test Engineer

**Purpose:** Independently verify expected behavior.

Responsibilities:
- Convert acceptance criteria into test cases.
- Detect available test frameworks and define applicable test profiles.
- Run unit, integration, E2E, regression and browser checks.
- Capture screenshots, console/page errors and failed network requests.
- Classify failures and identify reproducible steps.
- Verify fixes and disclose untested areas.

Outputs: Test plan, results, screenshots, traces, defects and acceptance summary.

### AG-10 — Security Analyst

**Purpose:** Review project and platform security boundaries.

Responsibilities:
- Identify exposed secrets, unsafe configuration, dependency vulnerabilities and common application risks.
- Review authorization, input validation, data flow and execution boundaries.
- Inspect smart-contract risks when applicable.
- Verify redaction, sandbox and approval behavior.
- Explain severity, evidence and remediation.

Outputs: Findings with severity, affected component, evidence, impact and remediation.

### AG-11 — Deployment and Operations Engineer

**Purpose:** Prepare and execute authorized deployment and maintenance work.

Responsibilities:
- Detect project runtime/build/deployment requirements.
- Compare compatible hosting options, plan limits and commercial-use terms.
- Prepare deployment configuration and environment-variable requirements.
- Verify deployment status and health.
- Diagnose failed deployments and recommend rollback/redeploy.
- Document operational maintenance steps.

Outputs: Deployment plan, configuration diffs, deployment evidence, operational report.

Production deployment and destructive infrastructure actions require approval.

## 3. Tool catalog

Tool names below describe logical operations. Exact API schemas and implementations are defined in later phases.

### T-01 — Project context tools

- Create/list/open/rename/archive project.
- Create/list/open conversation.
- Read selected attachments and project metadata.
- Record project-scoped settings.

Permission scope: Selected project only.

### T-02 — Research tools

- Search public web/documentation sources.
- Fetch an allowed public URL.
- Extract relevant text and source metadata.
- Return citations.

Constraints: No credentialed browsing using personal sessions; treat fetched content as untrusted; respect access restrictions and rate limits.

### T-03 — Model tools

- List configured models.
- Read capability metadata.
- Generate response.
- Stream response where supported.
- Check provider/model availability.
- Record usage/error metadata.

Constraints: Only AI gateway can access provider credentials; respect provider terms and quota.

### T-04 — GitHub tools

- List installed GitHub repositories.
- Read repository metadata, branches, commits, trees and file contents.
- Create task branch.
- Commit authorized changes.
- Open pull request.
- Read pull-request checks/status.

Constraints: Selected installation repositories only; granted permissions only; no force-push/default-branch write in normal agent workflow; handle rate limits.

### T-05 — Filesystem tools

- List project files.
- Read a scoped file.
- Create/update a scoped file.
- Move/rename a scoped file.
- Delete a task-scoped file when permitted.
- Generate and retrieve diffs.

Constraints: Canonicalize paths; reject traversal outside workspace; protect user changes; log all writes/deletes.

### T-06 — Terminal/execution tools

- Inspect runtime and package manifests.
- Run an approved command in a named workspace.
- Read command status and output.
- Start/stop a project process.
- Capture build/test artifacts.

Constraints: Isolated runner only; timeout/resource limits; redaction; no host secrets or arbitrary privileged host access.

### T-07 — Browser/QA tools

- Open a scoped preview URL.
- Navigate and interact with a page.
- Set viewport/device profile.
- Capture screenshot and trace.
- Read console/page errors and failed requests.
- Run configured Playwright tests.

Constraints: Separate browser profile; preview URL allowlist; prevent internal-network access; evidence must record exact tested revision and viewport.

### T-08 — Deployment tools

- Inspect deployment compatibility.
- List configured deployment targets.
- Prepare deployment configuration.
- Deploy after authorization.
- Read deployment status/logs.
- Verify health and URL.
- Roll back/redeploy when provider supports it.

Constraints: No production deployment without explicit approval; secrets stay in protected provider configuration; respect provider plan/terms.

### T-09 — Reporting tools

- Generate task report.
- Link artifacts, test runs, commits, PRs and deployment records.
- Export project/phase status.
- Update progress register after verified state changes.

Constraints: Reports must be generated from stored evidence and distinguish suggestions from executed outcomes.

## 4. Tool permission categories

| Category | Scope | Typical autonomy |
|---|---|---|
| Read | Read selected project/repository data | Usually allowed after project connection |
| Analyze | Model analysis of supplied/authorized context | Allowed in all modes |
| Research | Public sources and documentation retrieval | Allowed when requested/configured |
| Write workspace | Create/update files inside selected task workspace | Guided/Auto if enabled |
| Git branch write | Create task branch and commit task changes | Guided/Auto if enabled |
| Execute command | Run within isolated workspace | Guided/Auto under configured limits |
| Browser test | Navigate scoped preview and capture evidence | Guided/Auto under preview policy |
| External mutation | Create PR, deploy, change external resource | Explicit permission and operation-specific approval as configured |
| Administrative | Change repo/app/security/platform configuration | Separate owner-only approval |

The permission category is not a provider content filter. It is an authorization boundary for access to resources and side effects.

## 5. Agent handoff contract

A specialist handoff should contain:
- Task and step ID.
- Role and requested outcome.
- Relevant context references.
- Acceptance criteria.
- Known constraints and permission scope.
- Expected output schema.
- Evidence required.
- Status and unresolved questions.

The specialist returns:
- Findings/implementation summary.
- Files/resources inspected or changed.
- Tool operation references.
- Evidence references.
- Risks and assumptions.
- Acceptance results.
- Recommended next agent/action.

## 6. Orchestrator responsibilities

The orchestrator:
1. Creates the task plan.
2. Selects specialist roles and models.
3. Checks permissions before requesting a tool.
4. Invokes tools through the gateway.
5. Records state and evidence.
6. Stops at approval boundaries.
7. Schedules verification.
8. Decides whether to repair, block, or complete.
9. Produces the final evidence-backed report.

No specialist agent can independently grant itself new permissions.

## 7. Scope control

This catalog defines roles and tool families for the approved product. It does not authorize implementation during Phase 1. Exact tool schemas, provider APIs and runner commands are specified during the relevant implementation phases.
