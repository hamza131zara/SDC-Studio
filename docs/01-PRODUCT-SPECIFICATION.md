# SDC Studio — Product Specification

**Status:** Draft  
**Source baseline:** Approved Phase 0 Foundation  
**Product:** Private AI software-engineering workspace for Software Damage Control

## 1. Purpose

SDC Studio is a private, browser-based AI engineering workspace that coordinates model providers, specialist agent roles, repository tools, isolated project execution, browser testing, and evidence-based reporting.

An operator describes a software task in natural language, attaches relevant files or images, chooses an operating mode and autonomy level, connects a blank or existing repository, and supervises work through a visible task lifecycle.

The initial product is private and local-first, intended for agency engineering work. It can later support remote runners and additional deployment providers without changing the core product purpose.

## 2. Product outcomes

The product is successful when an operator can:
- Start a project from an idea or connect a selected existing repository.
- Request planning, documentation, design, development, testing, reverse-engineering, deployment, or maintenance assistance.
- See the agent plan, active step, tools used, files inspected or changed, commands, errors, test results, screenshots, and decisions.
- Choose configured AI providers/models and optional local models.
- Permit broad read/write/build/test work inside an explicitly authorized project workspace.
- Preview a running application and inspect screenshots, browser console errors, and failed network requests.
- Review exact diffs and recover from unwanted changes.
- Receive a report that distinguishes completed work, failed checks, blocked actions, and recommendations.

## 3. Intended operating context

### 3.1 Initial actors

- **Owner/operator:** Configures the private platform, connects repositories and providers, chooses execution permissions, reviews changes, and authorizes high-impact actions.
- **AI agent roles:** Logical engineering roles coordinated by the orchestrator. They are not independent users and do not receive credentials directly.
- **Project runtime:** Isolated workspace in which selected project code and approved commands execute.

The initial release is not a public multi-tenant SaaS product and does not require public customer onboarding or subscription billing.

### 3.2 Project types

The platform should not be limited to one framework. It should support discovery and engineering workflows for websites, web applications, APIs, mobile applications, DApps, smart contracts, LLM applications, and AI agents, subject to available runtime, model capabilities, hardware, credentials, and hosting target.

## 4. Product principles

### 4.1 Maximum practical user freedom

The target is **10/10 user freedom for legitimate, authorized engineering work**. SDC Studio must not add arbitrary topic bans, unnecessary technology allowlists, artificial friction, or repeated approvals for routine authorized actions.

The owner controls provider/model selection, project permissions, execution mode, and available tools. The system clearly distinguishes its own limitations from provider policy, provider quota, missing permissions, platform limitations, legal constraints, and necessary security boundaries.

Literal zero restrictions across third-party services cannot be guaranteed. SDC Studio does not bypass provider terms, GitHub controls, hosting restrictions, authentication, licensing, law, or safeguards protecting credentials and project data.

### 4.2 Transparent execution

The interface exposes what the system is doing. A model statement is not proof that a tool ran. Files changed, commands, test results, screenshots, commit IDs, and deployment statuses originate from tool evidence.

### 4.3 Recoverable repository changes

The default workflow reads and analyzes first, then changes an isolated task branch/worktree. It records a baseline, shows diffs, preserves user work, and avoids direct changes to the default branch.

### 4.4 Provider independence

Model providers are replaceable through a normalized gateway. Provider-specific limits, capabilities, data terms, and failures remain visible.

### 4.5 Isolated execution

Project code and dependencies are untrusted. They run outside the platform control plane in a constrained workspace.

## 5. Functional requirements

### 5.1 Conversations and projects

- **FR-CHAT-01:** Create and retain multiple conversations.
- **FR-CHAT-02:** Stream responses where the selected provider supports streaming.
- **FR-CHAT-03:** Accept text, supported files, and images with type and size validation.
- **FR-PROJ-01:** Create a project without a remote repository.
- **FR-PROJ-02:** Connect a repository explicitly selected by the owner.
- **FR-PROJ-03:** Keep project workspaces and task histories separate.
- **FR-PROJ-04:** Show connected repository, default branch, working branch, and baseline revision.

### 5.2 Modes and autonomy

- **FR-MODE-01:** Provide Auto, Planning, Documentation, Design, Development, Testing & QA, Reverse Engineering, and Deployment & Maintenance modes.
- **FR-MODE-02:** Keep operating mode independent from model selection.
- **FR-AUTO-01:** Provide Ask, Guided, and Auto execution settings.
- **FR-AUTO-02:** Apply configured tool permissions and approval gates consistently.
- **FR-AUTO-03:** Avoid repeated approval prompts for routine reversible actions already authorized within the selected workspace.
- **FR-AUTO-04:** Require explicit approval for defined high-impact actions.

### 5.3 Model gateway

- **FR-AI-01:** Maintain a configurable catalog of provider/model adapters.
- **FR-AI-02:** Let the owner choose a model or allow task-based routing.
- **FR-AI-03:** Record provider, model, latency, usage information when available, and failures.
- **FR-AI-04:** Respect provider rate limits and data terms.
- **FR-AI-05:** Handle unavailable models, quota errors, timeouts, and provider failures with bounded retry/fallback behavior.
- **FR-AI-06:** Support optional local inference through a compatible adapter.

### 5.4 Planning and orchestration

- **FR-AGENT-01:** Convert a request into a task plan with steps and acceptance criteria.
- **FR-AGENT-02:** Assign logical specialist roles appropriate to the task.
- **FR-AGENT-03:** Track task and step states.
- **FR-AGENT-04:** Route all mutations and commands through the tool gateway.
- **FR-AGENT-05:** Record tool invocation and result evidence.
- **FR-AGENT-06:** Stop or request approval when an action exceeds configured permissions.

### 5.5 Repository and execution

- **FR-REPO-01:** Inspect repository state before planning modifications.
- **FR-REPO-02:** Preserve original baseline and existing user changes.
- **FR-REPO-03:** Use task branches/worktrees and never force-push by default.
- **FR-REPO-04:** Show changed files and diffs.
- **FR-REPO-05:** Create commits and pull requests on authorized branches.
- **FR-EXEC-01:** Run project commands in a workspace isolated from the platform control plane.
- **FR-EXEC-02:** Record command, working directory, time, exit code, and redacted output.
- **FR-EXEC-03:** Enforce workspace, resource, and network boundaries.

### 5.6 Preview and quality

- **FR-QA-01:** Start a detected/configured development server in the isolated workspace.
- **FR-QA-02:** Show an embedded preview when the runtime can expose one safely.
- **FR-QA-03:** Run appropriate project checks and tests.
- **FR-QA-04:** Use browser automation to capture screenshots and report console/network/navigation errors.
- **FR-QA-05:** Associate evidence with a task and test run.
- **FR-QA-06:** Support a bounded diagnose-fix-retest loop.
- **FR-QA-07:** Report untested areas and unresolved failures explicitly.

### 5.7 Research, reporting, deployment, maintenance

- **FR-RES-01:** Retrieve relevant public technical documentation for research tasks and cite sources.
- **FR-REPORT-01:** Produce a task report with scope, files changed, commands, tests, evidence, unresolved issues, and next steps.
- **FR-DEPLOY-01:** Recommend compatible hosting based on architecture and runtime needs.
- **FR-DEPLOY-02:** Require authorization before production deployment.
- **FR-MAINT-01:** Support maintenance analysis and authorized code/dependency improvements.

## 6. Modes and expected behavior

| Mode | Primary output | Expected behavior |
|---|---|---|
| Auto | Coordinated lifecycle plan and verified result | Orchestrate relevant roles/tools within configured permissions |
| Planning | Requirements, options, architecture, task plan and acceptance criteria | No repository mutations unless separately authorized |
| Documentation | Product/technical documentation | Modify only requested documentation scope |
| Design | UX flows, wireframes, design guidance and UI plan | Design work; code changes follow execution permissions |
| Development | Implementation, diffs and build/test evidence | Work in isolated branch/workspace |
| Testing & QA | Test results, screenshots, diagnostics and defects | Run checks; repair only within permissions |
| Reverse Engineering | Authorized codebase/system analysis | Read/analysis first; modify only when requested |
| Deployment & Maintenance | Deployment plan and maintenance report | High-impact production actions require approval |

## 7. Non-functional requirements

- **NFR-SEC-01:** Secrets are not exposed to browser code or model prompts.
- **NFR-SEC-02:** Untrusted project code does not execute in the platform control plane.
- **NFR-TRACE-01:** Task actions and outcomes are auditable.
- **NFR-REC-01:** Repository changes are attributable and recoverable.
- **NFR-REL-01:** Failed provider/tool operations result in explicit states, not silent success.
- **NFR-REL-02:** Retries are bounded and respect provider/platform limits.
- **NFR-UX-01:** The operator can determine current task status and next required action.
- **NFR-EXT-01:** Providers and hosting adapters can be added without changing orchestrator core contracts.
- **NFR-COST-01:** Provider usage and free-tier limitations are visible where usage data exists.
- **NFR-EVID-01:** Test/deployment claims are backed by captured tool output.

## 8. Out of scope

- Guarantee of unlimited free AI, compute, hosting, or storage.
- Circumvention of third-party safeguards, quotas, access controls, or licensing.
- Direct agent modification of the repository default branch.
- Public SaaS onboarding, billing, or generalized tenant administration.
- Implementation of UI, providers, runner, preview, testing, or deployment during this documentation phase.

## 9. Requirement traceability

Functional requirement IDs are stable references for later implementation tasks and tests. A requirement may be changed only with a recorded reason and owner approval when it affects the approved foundation.
