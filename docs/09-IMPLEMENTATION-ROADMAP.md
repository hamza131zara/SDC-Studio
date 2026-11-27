# SDC Studio — Master Implementation Roadmap and Backlog

**Status:** Draft baseline for owner approval  
**Authority:** Approved Phase 0 Foundation  
**Purpose:** This is the authoritative phase sequence and implementation backlog. Use it to plan work, identify dependencies, and determine whether a phase is complete.

## 1. Roadmap rules

1. Follow the phase order below. Do not start the next phase until the current phase has been reviewed and accepted by the owner.
2. A phase may contain multiple pull requests, but its scope must remain within the phase definition.
3. Do not silently move work between phases. Record an explicit change when a dependency requires it.
4. Every implementation item must have an ID, description, dependency, expected evidence, and acceptance condition.
5. A task is not complete because code was generated. The acceptance condition must be verified.
6. Status labels mean:
   - **Not started:** No implementation work has begun.
   - **In progress:** Work is actively being performed.
   - **Blocked:** A dependency, permission, decision, or environment prevents progress.
   - **Ready for review:** Deliverables exist and checks/evidence are prepared.
   - **Accepted:** The owner has approved the phase exit criteria.
   - **Deferred:** Explicitly moved out of current scope with a reason.
7. Product-scope changes require owner approval and a recorded impact assessment.
8. The project status file and progress log must be updated whenever a phase/task changes state.

## 2. Phase overview

| Phase | Name | Status at this revision | Main deliverable |
|---|---|---|---|
| 0 | Foundation | Accepted and merged | Approved product and architecture foundation |
| 1 | Documentation | Accepted and merged; PR #2 | Frozen project documentation baseline; merge commit b3ebadd4065400b4155c15469b016520a692d987 |
| 2 | UI/UX Design | In progress; branch phase-2/ui-ux-design | Approved user flows, wireframes, design system and screen designs |
| 3 | Application Shell | Not started | Running web shell, core navigation, projects, conversations and settings |
| 4 | AI Gateway and Orchestration | Not started | Working multi-provider model gateway and task orchestrator |
| 5 | GitHub and Project Workspaces | Not started | Authorized repository selection, discovery, branches and controlled writes |
| 6 | Development and Execution | Not started | Isolated project runner, terminal, filesystem and task execution |
| 7 | Preview and QA | Not started | Live preview, browser diagnostics, screenshots and repair/retest loop |
| 8 | Deployment and Maintenance | Not started | Provider-backed deployment and maintenance workflows |

The statuses above are a record of the repository state at the date shown in PROJECT-STATUS.md. They must be updated as work proceeds.

## 3. Phase 0 — Foundation (accepted)

**Objective:** Agree what SDC Studio is, who it is for, what it must support, its architectural boundaries, security model, and phase sequence.

**Completed deliverables:**
- Product definition, goals, scope and non-goals.
- Core user-freedom target: 10/10 for legitimate, authorized engineering work.
- Operating modes and separate execution autonomy levels.
- Logical architecture and proposed technology baseline.
- Proposed repository folder structure.
- GitHub, branch, execution, data and security principles.
- Phase roadmap and acceptance checklist.

**Git record:** PR #1, merged to main on 2026-11-27. Merge commit: 0cbefebdbea5303d3a1928126e57f3c98ca21f8b.

**Exit:** Accepted by owner and merged. Do not reopen unless a material requirement or technical constraint requires a controlled foundation change.

## 4. Phase 1 — Documentation (accepted and merged)

**Objective:** Turn the approved foundation into detailed, searchable, traceable documentation that can guide implementation and preserve project memory.

**Acceptance record:** Owner approved on 2026-11-27. PR #2 merged to main with squash merge commit b3ebadd4065400b4155c15469b016520a692d987. The Phase 1 documentation baseline is frozen.

### Work items

| ID | Work item | Dependency | Deliverable / acceptance |
|---|---|---|---|
| P1-01 | Product requirements | Phase 0 | Product specification with functional/non-functional requirements and stable IDs |
| P1-02 | User workflows | P1-01 | Step-by-step workflows for new projects, existing repositories, modes, approvals, testing, reporting and deployment |
| P1-03 | System architecture | Phase 0, P1-01 | Component responsibilities, data flows, trust boundaries, runtime topology and architecture decisions |
| P1-04 | Module contracts | P1-03 | Clear responsibilities, inputs/outputs, event/error conventions and dependency boundaries |
| P1-05 | Conceptual data model | P1-01 to P1-04 | Entities, relationships, states, ownership and retention rules |
| P1-06 | Security specification | P1-03, P1-04 | Threat boundaries, credentials, repository permissions, sandboxing, approvals and audit controls |
| P1-07 | Engineering standards | P1-03, P1-06 | Coding, Git, testing, observability, dependency and release standards |
| P1-08 | Acceptance criteria and traceability | P1-01 to P1-07 | Requirement-to-document and requirement-to-verification mapping |
| P1-09 | Master roadmap and backlog | Phase 0 | Phase-by-phase tasks, dependencies, deliverables and exit gates |
| P1-10 | Project status and progress register | Phase 0 | Current state, completed work, pending work, blockers and next action |
| P1-11 | Decision log | Phase 0 and P1-03 | Accepted, provisional and pending decisions with rationale |
| P1-12 | Agent/tool catalog | P1-03, P1-04, P1-06 | Specialist responsibilities, permitted tool categories and boundaries |
| P1-13 | Testing and QA strategy | P1-01 to P1-07 | Test layers, evidence model, acceptance and repair-loop strategy |
| P1-14 | Integration strategy | P1-03, P1-06 | AI provider, GitHub, research and hosting integration boundaries |
| P1-15 | Documentation consistency review | P1-01 to P1-14 | Cross-reference, terminology, status and scope audit |
| P1-16 | Owner review and approval | P1-15 | Approved documentation PR and recorded phase acceptance |

### Phase 1 deliverables

- Product specification.
- User workflow specification.
- System architecture.
- Module contracts.
- Conceptual data model.
- Security specification.
- Engineering standards.
- Acceptance criteria and traceability.
- Master implementation roadmap.
- Project status and progress register.
- Decision log.
- Agent and tool catalog.
- Testing and QA strategy.
- Integration strategy.
- Documentation index.

### Exit criteria

- All P1 work items are complete or explicitly deferred with owner approval.
- All linked documents exist and cross-reference correctly.
- Requirement identifiers are stable and traceable.
- Phase status and pending work reflect actual GitHub state.
- No UI design or application implementation is mixed into this phase.
- Owner approves and merges the Phase 1 PR.

**Next phase after acceptance:** Phase 2 — UI/UX Design. Do not start it before the Phase 1 merge.

## 5. Phase 2 — UI/UX Design (in progress)

**Objective:** Define the actual product experience before coding the interface.

**Current branch:** phase-2/ui-ux-design. The information architecture, user journeys, screen wireframe specification, visual design system, interaction/responsive specification, and design acceptance criteria are drafted. The clickable prototype and owner design approval remain pending.

### Work items

| ID | Work item | Expected deliverable |
|---|---|---|
| P2-01 | Information architecture | Navigation map, page hierarchy and project/chat structure |
| P2-02 | Core user journeys | Flows for new project, new chat, repository connection, task execution, approvals and reports |
| P2-03 | Main application shell design | Sidebar, header, workspace layout and responsive behavior |
| P2-04 | Chat experience | Message states, streaming, composer, attachments, model/mode/autonomy selectors |
| P2-05 | Project workspace design | Files, diffs, terminal, task timeline, preview and QA surfaces |
| P2-06 | Execution transparency design | Activity log, tool events, approvals, status indicators and evidence |
| P2-07 | Settings and integrations design | Provider settings, GitHub connection, permissions and project configuration |
| P2-08 | Design system | Typography, colors, spacing, components, states and responsive rules |
| P2-09 | Interaction prototype | Clickable design covering primary workflows, authored in an editable design canvas |
| P2-10 | Design review and acceptance | Owner-approved designs and recorded exceptions |
| P2-11 | AI design workflow and integration architecture | Design-agent pipeline, native artifact model, Figma/Penpot adapter strategy, visual review and design-to-development handoff |

**Dependencies:** Phase 1 accepted documentation.

### Current work-item status

| ID | Status | Evidence / next action |
|---|---|---|
| P2-01 | Draft prepared | design/01-INFORMATION-ARCHITECTURE.md |
| P2-02 | Draft prepared | design/02-USER-JOURNEYS.md |
| P2-03 | Draft prepared | design/03-SCREEN-WIREFRAMES.md |
| P2-04 | Draft prepared | Composer, message stream, modes, models and autonomy states specified |
| P2-05 | Draft prepared | Project workspace tabs and screen layouts specified |
| P2-06 | Draft prepared | Activity, tool events and approval states specified |
| P2-07 | Draft prepared | Provider/GitHub settings and integration screens specified |
| P2-08 | Draft prepared | design/04-DESIGN-SYSTEM.md |
| P2-09 | Brief ready; prototype pending | Connect Penpot MCP in a supported MCP client, or use Product Design in ChatGPT Work mode; create editable screens and clickable user flows from design/08-PROTOTYPE-DESIGN-BRIEF.md |
| P2-10 | Pending prototype/visual review | Review editable prototype, final branding/navigation choices and accept complete package; theme/density preferences are confirmed |
| P2-11 | Draft prepared | design/07-DESIGN-AGENT-ARCHITECTURE.md; current design-tool access and free-first adapter strategy documented |

**Exit:** Main screens and key interactions are approved; responsive and error/empty/loading states are specified; the clickable design prototype and design handoff are ready; owner approves Phase 2.

**Not part of Phase 2:** Building backend APIs, connecting live AI providers, implementing GitHub, creating the actual execution runner, or writing production application code.

## 6. Phase 3 — Application Shell (not started)

**Objective:** Implement the approved visual experience and core application structure without prematurely building the full agent engine.

### Work items

| ID | Work item | Expected deliverable |
|---|---|---|
| P3-01 | Repository/app bootstrap | Monorepo, package management, TypeScript, lint/format and CI baseline |
| P3-02 | Web app foundation | Next.js application, layout, routing and responsive shell |
| P3-03 | Private operator access | Initial local/private authentication boundary and protected app routes |
| P3-04 | Database foundation | PostgreSQL connection, migration mechanism and repository pattern |
| P3-05 | Project management UI/API | Create, list, rename, archive and open projects |
| P3-06 | Conversation persistence | Create conversations, store messages and load history |
| P3-07 | Chat interface | Composer, streaming-ready interface, message rendering and error states |
| P3-08 | Attachments | Upload validation, metadata, storage reference and retrieval boundary |
| P3-09 | Settings shell | Provider/model settings structure, GitHub settings structure and autonomy settings UI |
| P3-10 | Task status event foundation | Persist task records and render state/event timeline |
| P3-11 | Application shell QA | Browser tests, responsive screenshots, accessibility and regression checks |

**Dependencies:** Phase 2 approved design.

**Exit:** The application runs locally, key shell workflows function, data persists, and tests pass. Provider orchestration and repository mutation remain later-phase work.

## 7. Phase 4 — AI Gateway and Orchestration (not started)

**Objective:** Connect free-tier cloud and optional local models behind a replaceable gateway and implement controlled task orchestration.

### Work items

| ID | Work item | Expected deliverable |
|---|---|---|
| P4-01 | Provider adapter contract | Normalized request, response, streaming, capability and error interfaces |
| P4-02 | Initial provider adapters | At least one verified provider; additional free providers added from approved candidates |
| P4-03 | Local model adapter | Optional Ollama-compatible local inference integration |
| P4-04 | Model catalog | Provider/model list, capability metadata and enabled/disabled state |
| P4-05 | Credential handling | Protected provider secret storage and redaction |
| P4-06 | Usage/quota tracking | Provider response usage, limits, latency and error records where exposed |
| P4-07 | Model routing | User-selected model and task-capability-based optional routing |
| P4-08 | Fallback handling | Bounded retry, rate-limit handling and alternative configured provider selection |
| P4-09 | Task state machine | Validated states, transitions, persistence and resume behavior |
| P4-10 | Planning agent | Request analysis, assumptions, steps and acceptance criteria |
| P4-11 | Logical specialist roles | Architect, researcher, designer, developers, QA and other approved roles |
| P4-12 | Tool gateway foundation | Typed tool requests/results, policy checks and audit events |
| P4-13 | Orchestration tests | Model adapter, routing, state transition, failure and audit tests |

**Dependencies:** Phase 3 shell, persistence and settings.

**Exit:** At least one provider works through the gateway, local fallback is available if configured, a prompt can produce a persisted plan, and orchestration behavior is covered by tests. This phase does not yet grant broad repository mutation or arbitrary command execution.

## 8. Phase 5 — GitHub and Project Workspaces (not started)

**Objective:** Connect selected repositories and enable controlled, visible source-code operations.

### Work items

| ID | Work item | Expected deliverable |
|---|---|---|
| P5-01 | GitHub App registration | App identity, callback/webhook configuration and requested permissions |
| P5-02 | Installation authorization | Owner installation flow and selected-repository scope |
| P5-03 | Token lifecycle | Short-lived installation token acquisition, refresh and revocation handling |
| P5-04 | Repository picker | Search/list only repositories available to the installation |
| P5-05 | Read-only discovery | Metadata, branch, commit, tree, manifests, docs and test detection |
| P5-06 | Baseline and status view | Record commit SHA and show default/current branch state |
| P5-07 | Local clone/fetch | Clone selected repository into the correct workspace |
| P5-08 | Safe task branch | Create task branch/worktree from verified baseline |
| P5-09 | File operations | Scoped read/write operations through the tool gateway |
| P5-10 | Diff and history | Show changed paths, diff, commit history and task attribution |
| P5-11 | Commit and pull request | Create commits and PRs on authorized task branches |
| P5-12 | Rate-limit and recovery | Handle 403/429, conflicts, timeouts and remote-state reconciliation |
| P5-13 | GitHub security tests | Verify repository scope, permission checks, branch safety and secret redaction |

**Dependencies:** Phase 4 tool gateway and Phase 3 project persistence.

**Exit:** The owner can select a repository, inspect it read-only, create a task branch, make an authorized change, inspect the diff, commit, and open a PR. Default branch remains untouched by normal task execution.

## 9. Phase 6 — Development and Execution (not started)

**Objective:** Allow agents to build and modify projects through an isolated runner.

### Work items

| ID | Work item | Expected deliverable |
|---|---|---|
| P6-01 | Runner lifecycle | Start, inspect, stop, resume and clean up a task workspace |
| P6-02 | Isolation baseline | Container/workspace configuration, non-root user and bounded resources |
| P6-03 | Filesystem tools | Scoped tree/list/read/write/move/delete with path validation |
| P6-04 | Command tools | Shell command execution with explicit cwd, timeout and exit code |
| P6-05 | Environment handling | Scoped environment injection and redaction |
| P6-06 | Dependency/install policy | Inspect package scripts and run install/build tools in isolation |
| P6-07 | Project detection | Detect framework, runtime, package manager and test/build scripts |
| P6-08 | Development loop | Plan, edit, run, inspect, repair and retest workflow |
| P6-09 | Resource and network controls | CPU/memory/disk/time limits and configured egress |
| P6-10 | Recovery and cleanup | Preserve baseline, capture state and clean temporary workspaces |
| P6-11 | Runner security tests | Path traversal, privilege, mount, secret and resource-limit tests |

**Dependencies:** Phase 4 tool contracts/orchestrator and Phase 5 workspace/repository operations.

**Exit:** A task can execute a permitted command, modify scoped files, record diffs/output, survive recoverable failures, and cannot access platform secrets or unrelated host files.

## 10. Phase 7 — Preview and QA (not started)

**Objective:** Provide a live project preview and reliable browser/test evidence.

### Work items

| ID | Work item | Expected deliverable |
|---|---|---|
| P7-01 | Project start/stop detection | Configurable and detected development server lifecycle |
| P7-02 | Preview routing | Scoped preview URL/proxy and readiness checks |
| P7-03 | Browser runner | Isolated Playwright browser lifecycle |
| P7-04 | Viewport profiles | Desktop, tablet and mobile viewport profiles |
| P7-05 | Screenshot capture | Named, revision-linked screenshots |
| P7-06 | Console/network capture | Console errors, page errors, failed requests and response diagnostics |
| P7-07 | Automated test integration | Unit, integration and E2E command/profile results |
| P7-08 | Accessibility/performance | Applicable audits and measured outputs |
| P7-09 | QA analysis | Failure classification and root-cause hypotheses |
| P7-10 | Repair/retest loop | Bounded permitted fixes and regression reruns |
| P7-11 | QA reporting | Test summary, coverage, evidence, unresolved issues and screenshots |
| P7-12 | Browser security tests | SSRF, internal endpoint, cookie and preview isolation checks |

**Dependencies:** Phase 6 working execution runner.

**Exit:** A supported application runs in preview, browser checks execute, screenshots and diagnostics are retained, test claims are evidence-backed, and repair/retest is bounded and reportable.

## 11. Phase 8 — Deployment and Maintenance (not started)

**Objective:** Publish supported projects through compatible providers and provide post-deployment guidance/maintenance.

### Work items

| ID | Work item | Expected deliverable |
|---|---|---|
| P8-01 | Deployment capability detection | Identify static, serverless, container and persistent-service requirements |
| P8-02 | Hosting recommendation | Compare compatibility, limits, commercial-use terms and cost |
| P8-03 | Provider adapter contract | Normalized deploy, status, logs, domains and rollback interface |
| P8-04 | First provider integration | Verified deployment to the selected initial provider |
| P8-05 | Environment/secret setup | Protected production variable management |
| P8-06 | Project naming and URLs | Provider subdomain and platform-owned subdomain mapping where supported |
| P8-07 | Custom domain support | Connect a domain controlled by the owner and validate DNS/TLS |
| P8-08 | Deployment verification | Health check, preview check and deployment record |
| P8-09 | Rollback/redeploy | Re-deploy prior successful artifact when provider supports it |
| P8-10 | Maintenance workflow | Dependency updates, incident analysis, health checks and authorized repairs |
| P8-11 | Deployment/security tests | Credential protection, target isolation, approval, status and rollback tests |

**Dependencies:** Phase 7 verified build/preview and Phase 6 execution. A limited deployment adapter may be tested against a compatible build before full QA integration only if separately approved.

**Exit:** At least one suitable target can be deployed with authorization, deployment evidence is recorded, and hosting limitations are explained. No universal free hosting guarantee is made.

## 12. Cross-phase dependencies

- Phase 1 documentation defines the contracts consumed by design and implementation.
- Phase 2 design is a prerequisite to building the UI shell.
- Phase 3 persistence and settings are prerequisites to robust model/task orchestration.
- Phase 4 policy/tool contracts are prerequisites to repository and runner operations.
- Phase 5 repository/workspace control is required for safe existing-repo development.
- Phase 6 runner isolation is required before project commands and builds are executed.
- Phase 7 QA depends on a functioning runner and preview lifecycle.
- Phase 8 deployment depends on project build knowledge, secrets handling, and deployment authorization.

## 13. Phase completion record

For each phase, record:
- Phase ID and name.
- Start and completion dates.
- Owner approval.
- Branch and pull request.
- Merge commit SHA.
- Work items completed/deferred.
- Test/evidence references.
- Known limitations.
- Authorized changes to future scope.

## 14. Roadmap change policy

Do not add phases or unrelated tasks for convenience. If an implementation dependency forces a change, propose the smallest adjustment, explain the reason and impact, and wait for owner approval before changing the roadmap.
