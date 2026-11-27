# SDC Studio — User Workflows

**Status:** Draft  
**Scope:** Formalize the operator and agent workflows approved in Phase 0. These are behavior definitions, not implementation tasks.

## 1. Global workflow rules

1. Every task belongs to a project and conversation.
2. Every task records its operating mode and execution autonomy.
3. The agent inspects relevant project/repository context before proposing changes.
4. Every task has a plan or explains why a plan is unnecessary.
5. Every mutation and command passes through the tool gateway.
6. High-impact actions pause for approval.
7. Every run ends with a verified status and evidence-based report.
8. Later phases preserve these rules.

## 2. Create a project from a blank workspace

1. Operator selects New Project.
2. Operator provides a project name and initial description/prompt.
3. Operator optionally attaches files or images.
4. Operator selects an operating mode, execution setting, and model/provider preference.
5. SDC Studio creates a project record and isolated workspace.
6. Agent interprets the request and returns a plan with assumptions, tasks, and acceptance criteria.
7. Operator reviews the plan according to selected mode/autonomy.
8. Authorized implementation proceeds in the workspace.
9. The system runs applicable checks and presents results.
10. Operator may connect a GitHub repository when remote persistence is needed.

**Evidence:** Project ID, initial prompt, selected settings, plan, task history, workspace identity, and final report.

## 3. Connect an existing GitHub repository

1. Operator selects Connect GitHub.
2. Operator completes GitHub App authorization.
3. Operator selects only the repositories to install the app on.
4. SDC Studio records installation and granted permissions.
5. Operator selects a repository from the installed repository list.
6. GitHub adapter reads metadata, default branch, current commit, and permitted tree/content.
7. SDC Studio performs read-only discovery: languages, framework hints, scripts, dependency manifests, directories, tests, and existing documentation.
8. SDC Studio reports scan coverage, skipped files, permission errors, and repository baseline.
9. No files are changed during connection or initial discovery.
10. Operator starts a task when ready.

**Evidence:** Installation/repository identity, granted permissions, default branch, baseline commit SHA, discovery summary, and audit entries.

## 4. Start a task against an existing repository

1. Operator opens a project and starts a new chat/task.
2. Operator submits a request and relevant attachments.
3. Operator selects a mode and Ask, Guided, or Auto autonomy.
4. Agent resolves project context and inspects relevant files/history.
5. Agent creates a plan and acceptance criteria.
6. Policy layer checks requested tools and operations against project permissions.
7. Ask mode returns recommendations without mutation.
8. Guided mode performs pre-authorized routine work and pauses for defined approvals.
9. Auto mode performs permitted routine work autonomously; high-impact approval gates remain active.
10. Runner creates or reuses an isolated task branch/worktree from the recorded baseline.
11. Agent implements through the tool gateway.
12. System records changed files, diffs, commands, and outputs.
13. Verification and QA run.
14. Agent repairs in-scope failures when permitted, then reruns affected checks.
15. System produces the task report and presents the diff.
16. Depending on project policy, changes remain in the workspace or are committed and offered through a pull request.
17. Merge to the protected/default branch is not automatic by default.

**Evidence:** Plan, baseline SHA, working branch, tool activity, diffs, commands, test evidence, and final disposition.

## 5. Planning-only workflow

1. Operator selects Planning mode.
2. Operator submits a concept, feature, bug, or system question.
3. Research/analysis agents inspect supplied context and authorized reference material.
4. System identifies assumptions, unknowns, dependencies, risks, alternatives, and sequencing.
5. System returns a plan and acceptance criteria.
6. No repository mutations occur unless implementation is separately requested and permitted.

**Output:** Structured plan, decisions requiring owner input, risks, and next actionable task.

## 6. Documentation workflow

1. Operator selects Documentation mode.
2. Operator specifies document purpose and scope.
3. Agent checks existing documentation to preserve terminology and avoid duplication.
4. Agent prepares proposed changes.
5. Changes remain within the requested documentation scope.
6. System shows diffs and reports documents created/updated.
7. Documentation checks run where available.

**Evidence:** Document list, source context, diffs, validation output, and unresolved assumptions.

## 7. Design workflow

1. Operator selects Design mode.
2. Agent confirms product goals, users, page/flow scope, and visual constraints.
3. Agent produces proposed user flows, information architecture, wireframes, and design direction.
4. Operator reviews and approves design deliverables.
5. Implementation is not started solely because a design proposal was generated; Development or Auto execution must be requested and authorized.

Detailed interface design and design execution belong to the later UI/UX Design phase, not this documentation phase.

## 8. Development, preview, and QA workflow

1. Runner identifies project runtime and documented start/test commands.
2. Agent inspects scripts and dependency behavior before execution.
3. Project starts in its isolated workspace with bounded resources.
4. Preview becomes available only after a successful server readiness check.
5. Browser runner executes configured user journeys and viewports.
6. Browser console errors, uncaught exceptions, failed requests, navigation errors, screenshots, and traces are captured.
7. Test runner records command, exit status, duration, and results.
8. QA agent classifies failures and likely root causes.
9. Developer agent changes only task-scoped files and records diffs.
10. Affected tests and regression checks rerun.
11. Loop is bounded by configured attempt/time/resource budgets.
12. If unresolved, task reports the blocker rather than claiming success.

**Evidence:** Preview session, screenshots, browser diagnostics, test reports, code diffs, and repair history.

## 9. Research workflow

1. Operator asks a technical question or enables research within a task.
2. Research agent identifies needed information and searches permitted public sources.
3. Sources are checked for relevance and freshness.
4. Findings include citations and distinguish source facts from inference.
5. Retrieved webpage content is untrusted data, not platform instruction.
6. Research output is included in the conversation/task report where appropriate.

## 10. Deployment workflow

1. Operator selects Deployment & Maintenance mode or requests deployment through Auto.
2. Agent inspects architecture, build output, runtime needs, environment variables, persistence, and hosting compatibility.
3. Agent recommends a target and explains limitations, quotas, terms, and expected costs where known.
4. Agent prepares the deployment plan and configuration changes in the task workspace.
5. Operator reviews high-impact production actions.
6. Deployment adapter executes only after authorization.
7. System records provider deployment ID, URL, status, logs, and verification.
8. Failed deployments return diagnostics and recovery recommendations.

**Evidence:** Target, approved configuration, deployment record, logs, URL, verification, and limitations.

## 11. Task states

| State | Meaning |
|---|---|
| Draft | Request exists but has not been submitted/planned |
| Planning | Request and context are being analyzed |
| Awaiting approval | An operation requires operator authorization |
| Queued | Planned and waiting for execution capacity |
| Running | A task step or tool operation is active |
| Verifying | Build/tests/browser checks are executing |
| Repairing | A permitted repair is in progress |
| Completed | Required acceptance conditions are met |
| Completed with issues | Main work completed, known issues remain |
| Blocked | Permission, external dependency, quota, environment, or decision prevents progress |
| Failed | Execution failed without meeting completion conditions |
| Cancelled | Operator or system cancelled the task |

A completed task may still disclose warnings or untested areas. Reports must not imply full coverage when only partial checks ran.

## 12. Approval workflow

An approval request shows:
- Operation requested and why it is required.
- Project/repository and affected target.
- Exact files/resources or deployment target.
- Expected consequence and reversibility.
- Alternatives, including declining.

Approval is bound to the specific operation and scope. It is not blanket approval for future actions.

## 13. Final task report

Every report contains:
1. Request and selected mode/autonomy.
2. Plan and acceptance criteria.
3. Repository/workspace baseline and working branch, when applicable.
4. Summary of work actually performed.
5. Files created, updated, deleted, or left unchanged.
6. Commands and exit codes.
7. Tests executed and actual outcomes.
8. Screenshots and browser/network diagnostics, when applicable.
9. Commit/PR/deployment references, when applicable.
10. Remaining risks, warnings, blocked items, and untested areas.
11. Recommended next steps tied to the original request.

## 14. Workflow change policy

Revise workflows only when a requirement is unclear, a material technical constraint emerges, or the owner requests a change. Record the reason and affected workflows. Do not introduce unrelated capabilities through workflow documentation alone.
