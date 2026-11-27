# SDC Studio — Core User Journeys

**Status:** Design draft  
**Work item:** P2-02  
**Purpose:** Specify the main interactions the UI must support, including normal, error, blocked, and approval states.

## 1. Journey A — Start a new chat without a project

**Goal:** Ask a general engineering/research question or begin a project idea.

1. Operator opens SDC Studio.
2. Home screen presents the composer.
3. Operator optionally selects mode, model, and autonomy; defaults are visible.
4. Operator types a request and optionally attaches files/images.
5. Operator submits.
6. Conversation is created and response streams.
7. If the request becomes a project task, the assistant offers a project association or project creation within the conversation.
8. Conversation appears in recent history.

**Success state:** Conversation is saved, selected mode/model/autonomy are visible, and the operator can resume later.

**Error states:** Attachment rejected, provider unavailable, quota exceeded, network interrupted, or no model configured. Preserve the draft and explain the actual error source.

## 2. Journey B — Create a blank project

1. Operator selects New Project.
2. Dialog asks for project name and optional description.
3. Operator chooses Blank Workspace.
4. Project overview is created.
5. Operator opens Chat and describes the application to build.
6. Agent returns plan and acceptance criteria according to selected mode.
7. Project appears in the sidebar.
8. Future conversations/tasks are linked to the project.

**Success state:** Project is created without requiring GitHub setup.

**Important distinction:** Creating a blank project does not imply that an external GitHub repository already exists.

## 3. Journey C — Connect a GitHub repository

1. Operator opens Integrations or project repository settings.
2. Selects Connect GitHub.
3. If the product GitHub App is not installed, operator is directed to GitHub authorization.
4. Operator selects only repositories to grant.
5. SDC Studio displays the actual installation scope and granted permissions.
6. Operator selects one repository.
7. Connection workflow performs read-only discovery.
8. UI displays repository name, owner, default branch, last commit, languages/framework hints, and scan status.
9. Operator confirms the repository is associated with the project.

**Success state:** Repository is connected read-only and baseline state is visible.

**Error states:** App not installed, repository not selected, permission insufficient, installation revoked, repository unavailable, or API rate limited. Do not silently broaden access.

## 4. Journey D — Give a development task to the agent

1. Operator opens the project.
2. Opens Chat and enters a task.
3. Selects Development or Auto mode.
4. Selects Ask, Guided, or Auto execution.
5. Selects model/provider or leaves routing automatic.
6. Adds supporting files/images if needed.
7. Submits.
8. Agent creates a plan with task steps and acceptance criteria.
9. The activity timeline shows analysis and tool use.
10. Before workspace changes, UI identifies repository baseline and task branch.
11. File changes, commands, and results appear as they occur.
12. Operator can inspect diffs during/after execution.
13. Agent reports test results and unresolved items.

**Success state:** The task is traceable from request to plan, operations, changes, tests, and report.

**Error/blocked states:** Missing model, quota exhausted, permission denied, branch conflict, sandbox unavailable, or approval pending. Preserve progress and show a resumable status.

## 5. Journey E — Review and approve a high-impact operation

1. Agent requests an action that requires approval.
2. Task changes to Awaiting approval.
3. UI shows operation, reason, project/resource, exact target, expected impact, and reversibility.
4. Operator chooses Approve or Reject.
5. If approved, only that specific operation is authorized.
6. If rejected, agent continues with an alternative where possible or marks task blocked.
7. The decision is stored in activity history.

**Success state:** Approval is explicit, scoped, and auditable.

**Important:** Do not interrupt routine authorized file reads or reversible edits with repeated prompts solely for appearance of safety.

## 6. Journey F — Inspect code changes

1. Operator opens Changes.
2. UI displays summary of modified, added, deleted, and renamed files.
3. Operator selects a file.
4. Diff viewer displays before/after changes.
5. Operator can inspect related task step, command, and test evidence.
6. When connected to GitHub, UI displays branch, commit, and pull request references.
7. Operator can request a revision or mark the changes ready for PR review.

**Success state:** Operator can identify exactly what changed and why.

## 7. Journey G — Open live preview and QA evidence

1. Operator opens Preview.
2. If application is not running, empty state explains the start action.
3. Agent/runner starts the project in the isolated workspace.
4. Readiness check confirms preview availability.
5. Preview appears in the workspace.
6. Operator may switch viewport.
7. QA run captures screenshots and browser diagnostics.
8. Operator opens QA to view test outcomes, console errors, failed requests, and screenshots.
9. Failed checks can be opened as tasks for diagnosis/repair.
10. A repair run records new evidence without overwriting the original failed run.

**Success state:** Preview and QA evidence refer to a known project revision.

## 8. Journey H — Review final report

1. Task reaches Completed, Completed with issues, Blocked, or Failed.
2. Final report appears in conversation.
3. Report includes summary, files changed, commands, tests, screenshots, errors, commit/PR/deployment references, and unresolved work.
4. Operator opens linked artifacts or diffs.
5. Report is stored in project history.
6. Suggested next steps are limited to direct dependencies or improvements relevant to the request.

**Success state:** Operator can resume the next task without reconstructing the prior session.

## 9. Journey I — Configure models/providers

1. Operator opens Settings → Models and Providers.
2. Sees configured providers and enabled models.
3. Adds or edits a provider credential through protected input.
4. UI displays connection/test status without revealing full credential.
5. Operator chooses default model or automatic routing.
6. Provider quota, unavailable state, or data-term notice is visible where available.
7. Operator can disable a provider or select a different configured model.

**Success state:** Model choice is explicit and does not change the selected operating mode or autonomy level.

## 10. Journey J — Review deployment recommendation

1. Operator opens Deployment & Maintenance mode.
2. Agent inspects project runtime/build needs.
3. UI displays compatible targets, free-plan constraints, expected costs where known, domain requirements, and unsupported capabilities.
4. Operator chooses a target.
5. Agent prepares the deployment plan.
6. Production deployment requires explicit approval.
7. Deployment status, URL, and logs appear in the project.

**Success state:** Operator understands where the application is deployed and what its hosting limitations are.

## 11. Cross-journey states

All journeys must define:
- Initial/empty state.
- Loading/in-progress state.
- Success/completed state.
- Recoverable error state.
- Permission-denied state.
- External provider/rate-limit state.
- Approval-required state.
- Cancelled state.
- Disconnected/reconnect state.

## 12. Journey acceptance

- Each primary journey has an identifiable entry point and completion state.
- Project context remains consistent when navigating.
- Error states preserve drafts and useful task context.
- Approval details are understandable before the operator decides.
- The agent's actual operations are discoverable from the journey.
- The UI does not claim a feature is connected/running when it is not.


## 13. Journey K — Design a new website/application with SDC Studio

**Goal:** Ask the SDC Studio design workflow to create an editable design and reviewable prototype before implementation.

1. Operator opens a project and selects Design mode or the Design workspace.
2. Operator provides a brief: product purpose, target users, platforms, primary tasks, required features and constraints.
3. Operator attaches brand assets, screenshots, sketches, documents or reference links.
4. The Design Agent summarizes the brief, identifies missing assumptions and creates design acceptance criteria.
5. UX Architect produces information architecture, screen inventory and user journeys.
6. Operator reviews the structure before high-fidelity design proceeds when the task is substantial.
7. Visual Designer creates low-fidelity wireframes.
8. Design Systems Specialist proposes typography, color/theme, spacing, components, variants and tokens.
9. Visual Designer creates high-fidelity editable screens and responsive states.
10. Prototype Specialist links screens into primary user journeys.
11. Design Critic checks requirement coverage, visual consistency, responsive behavior and accessibility.
12. The operator reviews the prototype, requests revisions and approves the design version.
13. SDC Studio prepares a handoff package for the later Development workflow.
14. The design artifact is saved to the project with its version, decisions, screens, tokens, reviews and approval state.

**Success state:** The project contains an approved, traceable, editable design package and a prototype. No production code is generated merely because the design has been approved.

**Fallback states:** If Figma/Penpot is unavailable, use the native design artifact model and preview. If vision-capable model access is unavailable, disclose that visual critique is limited. If required brand/reference input is missing, record the assumption instead of inventing brand rules.

## 14. Journey L — Revise an existing design

1. Operator selects an existing design version or connects a supported design file.
2. Agent reads available frames, components, variables and metadata under granted permissions.
3. Agent summarizes the current design and identifies the requested scope.
4. Agent proposes a focused change plan.
5. Agent updates only the affected screens/components/tokens.
6. Visual and responsive checks rerun for affected surfaces.
7. A new design version is recorded; the previously accepted version remains available.
8. Operator reviews and accepts/rejects the revision.

**Success state:** Design revision is traceable, prior accepted version remains recoverable, and the change report identifies exactly what changed.
