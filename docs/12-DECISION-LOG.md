# SDC Studio — Architecture and Product Decision Log

**Status:** Draft record for owner review  
**Purpose:** Preserve why major product/technical choices were made, distinguish approved decisions from provisional choices, and prevent future work from silently changing the foundation.

## 1. Decision status vocabulary

- **Accepted:** Approved in Phase 0 or explicitly accepted by the owner.
- **Provisional:** Current recommended baseline; must be validated before irreversible implementation.
- **Pending:** Requires an owner decision or evidence during a later phase.
- **Superseded:** Replaced by a newer decision; retain the record and link to the replacement.
- **Rejected:** Considered and intentionally not selected.

## 2. Accepted foundation decisions

### ADR-001 — Private agency-first product

**Status:** Accepted in Phase 0  
**Decision:** Build SDC Studio initially as a private tool for the agency's own engineering workflows, not a public multi-tenant SaaS.

**Reason:**
- The initial goal is an effective internal engineering platform.
- Public signup, billing, customer tenant boundaries, and commercial operations would expand scope before the core product works.
- Private use allows the team to validate the workflow first.

**Consequences:**
- Initial user model is owner/operator oriented.
- Authentication must still protect access, but public account lifecycle and billing are not Phase 1/initial implementation scope.
- Multi-tenant SaaS can be reconsidered only through an approved scope change.

### ADR-002 — Local-first initial execution

**Status:** Accepted in Phase 0  
**Decision:** Start with project execution on the owner's local machine through a separate runner, while keeping the browser-facing interface local/private.

**Reason:**
- Avoid mandatory paid cloud compute during the initial stage.
- Keep project workspaces under the operator's control.
- Support local Git, terminals, and browser testing.

**Consequences:**
- Local setup and resource limitations must be documented.
- The platform control plane and project execution remain separate logical/security boundaries.
- A hosted web/API and remote workers can be added later without granting project containers access to platform secrets.

### ADR-003 — Modular monolith plus isolated execution workers

**Status:** Accepted in Phase 0  
**Decision:** Use a modular monolith for the first platform release and separate project execution into isolated workers/workspaces.

**Reason:**
- Lower initial operational complexity than microservices.
- Clear boundaries are still possible through modules and typed contracts.
- The execution runner needs stronger isolation than ordinary application modules.

**Consequences:**
- Avoid premature distributed services.
- Preserve independent interfaces around AI, GitHub, execution, browser QA, and deployment.

### ADR-004 — Provider-independent AI gateway

**Status:** Accepted in Phase 0  
**Decision:** All model-provider access is routed through a replaceable adapter/gateway layer.

**Reason:**
- Free tiers, model availability, quotas, capabilities, and terms can change.
- Different tasks may benefit from different models.
- Paid providers may be added later without rewriting orchestration.

**Consequences:**
- Provider-specific credentials remain in the gateway.
- Capability, quota, data-term, and error metadata must be visible.
- Exact providers and initial model choices remain provisional until tested in Phase 4.

### ADR-005 — GitHub App with selected repositories

**Status:** Accepted in Phase 0  
**Decision:** Use a dedicated GitHub App and install it only on repositories explicitly selected by the owner.

**Reason:**
- More controlled repository access than a broad personal token.
- Permissions can be scoped to the required functions.
- Operations can be attributed and audited.

**Consequences:**
- App registration, permissions, callback, webhooks, installation lifecycle, and token handling are Phase 5 work.
- ChatGPT's existing GitHub connector is not the SDC Studio product's own GitHub App.
- Never store GitHub passwords or expose private keys to models.

### ADR-006 — Task branches/worktrees and protected default branch

**Status:** Accepted in Phase 0  
**Decision:** Agent tasks operate on isolated branches/worktrees and do not directly modify the default branch in ordinary workflows.

**Reason:**
- Preserve original code and user changes.
- Make changes inspectable and attributable.
- Support safe review and recovery.

**Consequences:**
- Capture baseline commit SHA.
- Show diffs and operation history.
- No force-push or history rewrite as normal agent behavior.
- Merge remains a separately governed operation.

### ADR-007 — Evidence-backed status and reports

**Status:** Accepted in Phase 0  
**Decision:** A task can be marked complete only from actual tool/test evidence and its defined acceptance conditions.

**Reason:**
- Model-generated statements alone are not proof of execution.
- The owner needs accurate progress and reliable continuation.

**Consequences:**
- Store commands, exit codes, test results, screenshots, diagnostics, diffs, commits and deployment references.
- Distinguish passed, failed, skipped, blocked and not-run work.

### ADR-008 — Maximum practical user freedom

**Status:** Accepted in Phase 0  
**Decision:** Target 10/10 freedom for legitimate, authorized engineering work, without arbitrary SDC-specific topic bans or unnecessary routine approval friction.

**Reason:**
- The tool is intended to be a general-purpose engineering assistant for the agency.
- The owner should control models, tools, authorized repositories, and execution autonomy.

**Consequences:**
- Provide configurable modes, autonomy levels, provider choice and project/tool permissions.
- Explain external provider, GitHub, hosting, legal, and security constraints accurately.
- Literal zero restrictions across third-party services cannot be guaranteed; SDC Studio must not bypass their controls.
- Security boundaries protect the owner's own machine, secrets, data, and projects.

### ADR-009 — Playwright for browser automation

**Status:** Accepted in Phase 0 baseline  
**Decision:** Use Playwright as the initial browser automation candidate.

**Reason:** It supports repeatable browser journeys, viewport testing, screenshots, and diagnostics.

**Consequence:** Validate the exact browser/runtime packaging during Phase 7 implementation.

### ADR-010 — PostgreSQL persistence

**Status:** Provisional baseline in Phase 0  
**Decision:** PostgreSQL is the proposed primary relational database.

**Reason:** Project, conversation, task, approval, tool, test, artifact, and audit records have relational ownership and consistency requirements.

**Consequence:** Confirm local setup and deployment footprint during Phase 3. Do not introduce another database without a concrete requirement.

## 3. Provisional technology decisions

These choices are the current baseline, not an excuse to ignore implementation evidence.

| Decision | Current baseline | Validation phase |
|---|---|---|
| Web app | Next.js + TypeScript | Phase 3 |
| UI components | Tailwind CSS + shadcn/ui | Phase 2/3 |
| Backend runtime | Node.js + TypeScript | Phase 3 |
| Package manager | pnpm workspace | Phase 3 |
| Agent orchestration | Custom typed state machine | Phase 4 |
| Model API compatibility | Provider-specific adapters; OpenAI-compatible interface where suitable | Phase 4 |
| Runner | Docker-based local runner | Phase 6 |
| Browser automation | Playwright | Phase 7 |
| Realtime updates | Server-Sent Events initially | Phase 3/4 |
| Hosting candidate | Cloudflare Pages/Workers for compatible apps | Phase 8 |

A change must document the problem with the baseline, alternatives, cost and complexity, security impact, compatibility, and owner approval.

## 4. Pending decisions

| ID | Decision needed | Why not decided now | Target phase |
|---|---|---|---|
| DEC-P01 | Exact initial cloud AI providers and models | Free availability, data terms, quotas, and coding quality must be checked at integration time | Phase 4 |
| DEC-P02 | Exact local Ollama model(s) and hardware profile | Depends on measurable quality and local resource limits | Phase 4 |
| DEC-P03 | Final GitHub App permissions/events | Define only against the features being implemented | Phase 5 |
| DEC-P04 | Exact container isolation profile and network policy | Must be validated against actual project runtimes and host OS | Phase 6 |
| DEC-P05 | Preview routing and local-browser isolation design | Depends on runner topology and security tests | Phase 7 |
| DEC-P06 | First production hosting provider | Must be selected against supported application type, free-plan terms, and runtime needs | Phase 8 |
| DEC-P07 | Platform-owned domain and wildcard routing | Requires ownership of a base domain and deployment architecture | Phase 8 |
| DEC-P08 | Exact database deployment mode | Depends on local setup and eventual hosted topology | Phase 3 |

Do not make these decisions prematurely merely to fill a document. Record evidence and owner approval when they are made.

## 5. Rejected approaches

### R-001 — One fixed AI provider

Rejected because the product requires model choice and resilience to changing free-tier availability.

### R-002 — Direct agent writes to main

Rejected because it risks overwriting working code and bypasses review/recovery.

### R-003 — Execute repository commands inside the web/API process

Rejected because untrusted project code must not share the platform control plane.

### R-004 — Unlimited retries for provider/GitHub failures

Rejected because it can create rate-limit enforcement, duplicate operations, and uncontrolled costs. Use bounded retry and reconcile state.

### R-005 — Promise universal zero restrictions or unlimited free hosting/AI

Rejected because external service limits, laws, security boundaries, and commercial terms cannot be controlled by SDC Studio.

## 6. Decision update procedure

For every new decision:
1. Assign the next stable ADR or DEC ID.
2. Record status and date.
3. State context/problem and considered alternatives.
4. Record the selected decision and rationale.
5. Document consequences and affected requirement/roadmap IDs.
6. Identify which phase verifies or implements it.
7. Obtain owner approval for material scope/architecture changes.
