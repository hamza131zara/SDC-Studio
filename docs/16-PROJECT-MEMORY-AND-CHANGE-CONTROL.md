# SDC Studio — Project Memory and Documentation Operating Guide

**Status:** Draft  
**Purpose:** Make this repository the durable source of truth for project direction, architecture, requirements, decisions, progress, blockers, and next actions.

## 1. Why this document exists

Project continuity must not depend on a single chat, assistant session, browser history, or a person's memory. All decisions and progress that affect future work must be recorded in the repository.

When resuming work after a break, a new chat, a context reset, or a change of assistant/tool, use this documentation system to reconstruct the current state.

## 2. Source-of-truth hierarchy

When two sources disagree, use this order and resolve the conflict explicitly:

1. **Owner's latest explicit instruction:** The current project owner's direct approval or requested correction takes precedence over older notes.
2. **Approved Phase 0 foundation on main:** Defines product identity, goals, scope, freedom principle, security model, architecture baseline, and original phase sequence.
3. **Approved phase documentation on main:** Defines detailed requirements, workflows, module boundaries, data model, engineering standards, and accepted criteria.
4. **Project Status Register:** Defines current progress, blockers, pending actions, and immediate next action. Verify its claims against GitHub before acting.
5. **Master Implementation Roadmap:** Defines phase order, work item IDs, dependencies, deliverables, and exit conditions.
6. **Decision Log:** Defines why an architectural/product choice was accepted, provisional, pending, rejected, or superseded.
7. **Open pull requests and branch commits:** Define proposed changes not yet accepted into main.
8. **Conversation history:** Useful context, but not the authoritative project record when repository state is available.

Never silently resolve a conflict by guessing. Record the discrepancy, inspect the relevant files/commits, and ask the owner only when a material decision cannot be derived from an approved source.

## 3. Authoritative document map

| Need | Open this document |
|---|---|
| Product purpose, requirements and scope | 01-PRODUCT-SPECIFICATION.md |
| How the operator and agents work | 02-USER-WORKFLOWS.md |
| System components and data flows | 03-SYSTEM-ARCHITECTURE.md |
| Module ownership and interfaces | 04-MODULE-CONTRACTS.md |
| Database entities and relationships | 05-DATA-MODEL.md |
| Permissions, secrets, isolation and threat boundaries | 06-SECURITY-SPECIFICATION.md |
| Coding, Git, testing and release standards | 07-ENGINEERING-STANDARDS.md |
| Phase acceptance and verification | 08-ACCEPTANCE-CRITERIA.md |
| Phase sequence, backlog and dependencies | 09-IMPLEMENTATION-ROADMAP.md |
| Current progress and next action | 10-PROJECT-STATUS.md |
| Requirement IDs and verification mapping | 11-REQUIREMENTS-TRACEABILITY.md |
| Why decisions were made | 12-DECISION-LOG.md |
| Specialist agents and available tool families | 13-AGENT-AND-TOOL-CATALOG.md |
| Testing, browser QA and evidence | 14-TESTING-AND-QA-STRATEGY.md |
| AI, GitHub, research and hosting integration boundaries | 15-INTEGRATION-STRATEGY.md |
| How to resume and update project memory | This document |

The Phase 0 foundation document remains at repository root as the approved original baseline.

## 4. Resume procedure for a future work session

Before making changes:

1. Confirm repository identity: hamza131zara/SDC-Studio.
2. Check the default branch and current branch.
3. Inspect open pull requests, especially the current phase PR.
4. Compare the active phase branch against main.
5. Open the Project Status Register and verify its commit/PR references.
6. Open the Master Roadmap to identify the active phase and eligible work items.
7. Read the relevant detailed specification before implementing a task.
8. Read the Decision Log for accepted and pending choices.
9. Check whether the owner has supplied a newer instruction or approval.
10. Summarize the verified state internally before acting: completed, in progress, blocked, pending, and next permitted action.

Do not assume that a phase PR has merged merely because a prior conversation said it was ready. Verify its GitHub merged state and merge commit.

## 5. Work item lifecycle

Each meaningful roadmap item follows this lifecycle:

1. **Not started:** No work has been performed.
2. **Ready:** Dependencies and scope are satisfied; work can begin in the current phase.
3. **In progress:** A branch/task is actively being changed.
4. **Blocked:** A dependency, external service, missing decision, or permission prevents progress.
5. **Ready for review:** Deliverables and required evidence are present.
6. **Accepted:** Owner has accepted the work/phase and the required merge has occurred.
7. **Deferred:** Owner approved moving the item out of scope with a reason.

Do not use "complete" for work that only has a draft, generated code, or an unverified claim. Use "ready for review" until owner approval is recorded.

## 6. Required progress update after work

After each material work session or merged PR, update:

- Current phase and status.
- Completed work item IDs.
- In-progress work item IDs.
- Blocked items and reasons.
- Pending owner decisions.
- Branch and PR references.
- Latest verified head/merge SHA.
- Test/evidence state.
- Immediate next action.
- Dated progress-history entry.

Update the Project Status Register first. Update the Roadmap if work-item status/dependencies changed. Update the Decision Log if a decision was added or changed. Update the Requirement Traceability Matrix if a requirement or verification mapping changed.

## 7. Phase gate procedure

A phase is not accepted solely because its files have been created.

Before asking for phase approval:
1. Compare actual deliverables against the phase exit criteria.
2. Verify every required file and internal link.
3. Verify requirement IDs and work item references.
4. Check documents for contradictions or outdated statements.
5. Verify GitHub branch, PR, commits, and changed file list.
6. Run only applicable checks and state clearly which checks were not applicable.
7. Prepare a review summary with completed items, exceptions, and unresolved issues.
8. Keep the PR open until the owner approves it.
9. After owner approval, merge using the approved repository method.
10. Verify the merge on main and record the merge SHA.
11. Update the Project Status Register and progress history.
12. Only then begin the next phase.

## 8. Scope and roadmap change control

Do not add unrelated features, speculative integrations, or extra phases. A change to scope or architecture is justified only when:
- A requirement was omitted or misunderstood.
- A technical constraint makes the approved choice infeasible.
- A security/reliability problem requires a change.
- The owner explicitly requests a change.

A proposed change records:
- Reason and evidence.
- Affected requirement and roadmap IDs.
- Alternative choices considered.
- Scope/cost/security/reliability impact.
- Affected documents and implementation phases.
- Whether owner approval is required.

Do not implement the change before required approval.

## 9. Decision and assumption discipline

Clearly label statements:
- **Approved decision:** Owner accepted it or it is in the approved foundation.
- **Provisional baseline:** Recommended but requires validation.
- **Assumption:** Used temporarily because evidence is unavailable.
- **Pending decision:** Requires future evidence or owner input.
- **External constraint:** Imposed by an AI provider, GitHub, host, operating system, or law.
- **Verified fact:** Supported by current repository/tool output or a cited current external source.

Do not turn a provisional provider, framework, model, or host into a permanent commitment without evidence and approval.

## 10. AI assistant behavior when working in this repository

Any assistant or agent contributing to SDC Studio must:

- Read the relevant repository documentation before acting.
- Follow the current approved phase and work item scope.
- Verify repository state before writing.
- Work on a phase/task branch rather than changing main directly.
- Show or summarize changes and preserve user work.
- Keep credentials out of source files, prompts, and logs.
- Use only authorized repository and execution scopes.
- Record actual commands and test results.
- Never invent commits, test outcomes, screenshots, deployment URLs, or progress.
- Update status/progress files after material work.
- Stop before beginning a later phase.
- Ask the owner for decisions only when the documentation and available evidence do not resolve a material ambiguity.
- Avoid proposing extra unrelated implementations at the end of every phase.

The product's user-freedom target is 10/10 for legitimate, authorized engineering work. Do not create arbitrary SDC-specific topic restrictions; do not promise literal zero restrictions across external providers/platforms or bypass their controls.

## 11. Current project pointer

For the current state, always read **10-PROJECT-STATUS.md** first and verify its GitHub references. The status file must identify exactly one immediate next action. The detailed task list and phase dependencies are in **09-IMPLEMENTATION-ROADMAP.md**.

## 12. Document maintenance

- Keep documents focused; avoid duplicating entire specifications in several files.
- Link to the authoritative source instead of copying it.
- Keep document titles, IDs, phase names, and terminology consistent.
- Include dates for progress and approval records.
- When a document changes materially, update the documentation index if needed.
- Treat merged documentation as the approved baseline; changes to it require a focused PR and recorded rationale.
