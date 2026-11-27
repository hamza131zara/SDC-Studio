# SDC Studio — Interaction and Responsive Specification

**Status:** Design draft  
**Work items:** P2-03, P2-04, P2-05, P2-06, P2-07, P2-09  
**Purpose:** Define component behavior, responsive adaptation, status presentation, and prototype interactions.

## 1. Interaction principles

- Every control must have an understandable purpose and observable result.
- Async actions show progress and preserve user context.
- Disabled actions explain why they are unavailable when the reason is not obvious.
- Error messages identify the failure source and a useful recovery action.
- Do not show success until the underlying operation reports success.
- Keep actions close to the object they affect.
- Use confirmation only for consequential/destructive operations; avoid repetitive friction for routine work.
- Maintain project/task context while moving between Chat, Files, Changes, Preview, Terminal, QA, Activity, and Reports.

## 2. Responsive layout behavior

The following breakpoints are proposed design tokens and can be tuned after prototype review:
- **Wide desktop:** 1440px and above.
- **Desktop:** 1024px–1439px.
- **Tablet:** 768px–1023px.
- **Mobile:** below 768px.

These values are design guidance, not implementation code.

### Wide desktop
- Sidebar fixed/collapsible.
- Main workspace occupies flexible width.
- Context panel can remain open.
- Chat plus project context or IDE split panes may appear concurrently.
- Code diff can use split view where enough width exists.

### Desktop
- Sidebar remains visible or collapsed.
- Context panel opens as a narrower side panel.
- Default workspace should prioritize either conversation or active tool surface; do not force three narrow columns.
- Diff defaults to unified view if split view becomes cramped.

### Tablet
- Sidebar becomes a drawer or compact rail.
- Context panel becomes a drawer.
- Workspace tabs may scroll horizontally.
- Preview, files, terminal, and QA surfaces occupy the primary workspace one at a time.
- Keep chat composer accessible.

### Mobile
- Navigation is a full-height drawer.
- Context details open in a sheet/drawer.
- Chat is the primary interaction.
- File diffs, terminal, preview, QA and reports each occupy a dedicated view.
- Avoid compressed split panes.
- Approval cards and actions remain full-width and readable.
- Upload actions support the platform's available file picker/camera/photo chooser behavior without assuming a specific device API.

## 3. Sidebar interactions

- New Chat starts a fresh conversation without deleting prior history.
- New Project opens the project creation flow.
- Selecting a conversation restores the correct project/mode/task context.
- Selecting a project opens the project overview.
- Collapse/expand does not change route.
- Project/conversation search filters list content.
- Archived projects remain recoverable according to product retention settings.
- Current navigation item is indicated through more than color alone.

## 4. Chat composer interactions

### Input
- Multi-line text.
- Enter submits only if the chosen keyboard convention allows; Shift+Enter adds a newline. The exact default is finalized in prototype review.
- Paste/drop attachments only when supported and validated.
- Remove individual attachments before submit.
- Show file name/type/size and upload/validation status.
- Preserve unsent draft on recoverable provider/network errors.

### Submission
- Send button shows a pending/disabled state while request creation is in progress.
- After successful task creation, message appears and task state is visible.
- Streaming response shows a stable stop/cancel action when the provider/runtime supports cancellation.
- If streaming disconnects, show interruption and provide retry/resume only when safe.
- Prevent duplicate task creation from repeated clicks/submissions.

### Selectors
- **Mode selector:** Auto, Planning, Documentation, Design, Development, Testing & QA, Reverse Engineering, Deployment & Maintenance.
- **Model selector:** Configured provider/model or automatic routing.
- **Autonomy selector:** Ask, Guided, Auto.
- These selectors remain independent and must show the current effective values.
- If a model cannot support the selected task capability, show the reason and available alternatives; do not silently change privacy/data terms.

## 5. Task plan and activity interactions

- A plan can be collapsed/expanded.
- Steps display order, status, and short description.
- Selecting a step opens associated activity/evidence.
- Tool events expose safe details: tool name, purpose, target scope, status, and result.
- Full output is expandable and redacted.
- Long output uses a scrollable panel with copy action where safe.
- Running activity updates without jumping the user's scroll position.
- New activity can show an unseen-event indicator while the user reads older events.
- Task status and current step remain accessible near the conversation header.

## 6. Approval interactions

Approval card contains:
- Exact operation.
- Resource/repository/target.
- Reason.
- Scope of effect.
- Impact and reversibility.
- Related diff/plan/log when available.
- Approve and Reject actions.

Behavior:
- A decision applies only to the identified operation/scope.
- Show pending, approved, rejected, expired, and execution-result states.
- Approval cannot be silently reused for a different target or changed operation.
- Rejecting does not discard prior task progress.
- If approval expires, the agent requests a fresh decision only when the operation is still needed.

## 7. Project workspace tab interactions

- Tabs preserve their own local scroll/selection when safe and when the project context has not changed.
- Switching projects clears prior project's ephemeral preview/terminal content.
- Files selection highlights the active path.
- Changes selection stays linked to the active task.
- Preview indicates running revision and session state.
- Terminal shows workspace identity and command execution status.
- QA lists test runs chronologically.
- Activity is append-only and filters by agent/tool/status.
- Reports are immutable snapshots or versioned records; later tasks create new reports rather than rewriting an old result.

## 8. Preview and browser interactions

- Preview toolbar has reload, viewport selector, open separately, capture screenshot, and stop controls where supported.
- Current preview origin and source revision are visible.
- Preview loading shows readiness status.
- Expired/stopped preview shows a clear restart action.
- Screenshot capture reports success only after an artifact is stored.
- Browser QA launch identifies the selected test profile and target revision.
- Console and network panels use filters for errors, warnings, request failures, and status codes.
- Clicking a diagnostic item can navigate to its related screenshot/trace/test step where evidence exists.

## 9. Terminal interactions

- Command input is available only when execution permission is enabled.
- User sees which project workspace and current directory are active.
- Running command can be cancelled only where the runner supports safe cancellation.
- Exit code and duration are always visible after command completion.
- Copy output excludes secrets after redaction.
- Unknown/destructive command risks are handled by the execution policy, not by misleading visual warnings alone.

## 10. Settings and integration interactions

### Provider setup
- Show configured, disabled, unavailable, rate-limited, and authentication-error states.
- Credential input is masked.
- After save, never reveal the full saved credential.
- Test connection reports actual tool result and timestamp.
- Data terms/acknowledgement is shown when required by the provider integration design.

### GitHub
- Show connected account/installation and repository selection scope.
- Provide explicit disconnect/revoke path.
- Do not imply that ChatGPT's external connector is the SDC Studio GitHub App.
- Repository selection displays granted permissions in plain language.

### Execution settings
- Separate autonomy defaults from per-tool permission categories.
- Explain high-impact action approval boundaries.
- Show project-specific overrides where supported.
- Prevent ambiguous controls such as one switch labelled "unrestricted mode" that hides its actual permission effect.

## 11. Empty/loading/error state matrix

| Surface | Empty | Loading | Error/blocked |
|---|---|---|---|
| Home chat | Prompt composer and recent items | Conversation creation/stream status | Preserve prompt; identify provider/network error |
| Projects | Create project/connect repository | Project save/load | Explain persistence or access failure |
| GitHub | Connect GitHub action | Installation/repository scan | Permission, installation, rate-limit or unavailable state |
| Files | No repository/workspace yet | Tree/file loading | Missing file, permission denied, or unavailable |
| Preview | Start project workflow | Server readiness | Build failure, runner unavailable, preview expired |
| Terminal | No commands yet | Command running | Non-zero exit, timeout, denied command |
| QA | No tests run | Test profile running | Failed, skipped, blocked, or not configured |
| Activity | No task activity | Task queued/running | Tool failure or task blocked |
| Reports | No reports yet | Report generation | Artifact storage or report generation failure |
| Providers | No provider configured | Connection check | Bad credential, unavailable model, quota exhausted |

## 12. Keyboard and accessibility interaction

- All actions are keyboard reachable.
- Focus order follows visual reading order.
- Focus is not lost when task state updates.
- Dialog opening moves focus to the dialog; closing returns focus to the trigger.
- Escape closes non-destructive popovers/dialogs where appropriate.
- Keyboard shortcuts, if added, must be discoverable and non-conflicting with text editing.
- Screen-reader labels identify icon-only controls.
- Status changes use polite live regions; repeated streaming tokens must not flood announcements.
- Do not trap keyboard focus inside non-modal panels.

## 13. Reduced motion and sensory considerations

- Respect reduced-motion preference.
- No flashing or continuously animated decorative elements.
- Avoid sound-based-only alerts.
- Provide clear text labels for statuses.
- Allow comfortable reading density.

## 14. Prototype interactions required

The clickable design prototype should demonstrate:
1. New chat and send prompt.
2. New project and blank workspace selection.
3. GitHub connect/select repository concept.
4. Mode/model/autonomy selectors.
5. Task plan with expandable activity.
6. Approval request accept/reject.
7. Project workspace tab navigation.
8. Files and diff review.
9. Preview and responsive viewport selection.
10. QA test results and screenshot gallery.
11. Terminal state presentation.
12. Settings/provider setup.
13. Final task report navigation.

The prototype may use static sample content. It must be labelled as prototype data and must not imply a real model call, repository operation, test, or deployment.

## 15. Interaction acceptance

- Every visible control has a defined behavior.
- Async operations have pending/success/failure states.
- User drafts and context survive recoverable errors.
- Modes, models, and autonomy remain independent.
- High-impact approvals are specific and auditable.
- Workspace context does not leak across projects.
- Responsive layouts preserve task clarity and readability.
- Accessibility and reduced-motion behavior are defined.


## 16. Design workspace interactions

- The design stage list shows Draft, Running, Needs review, Approved, Blocked and Not started.
- Selecting a stage opens its artifacts and acceptance checklist.
- Operator can comment on a screen/frame or a specific finding.
- Request revision must identify the target screen/component and desired outcome.
- Theme and density selectors are independent:
  - Theme values: Light and Dark.
  - Initial theme follows the operating-system preference unless the operator chooses a theme.
  - A manual selection is remembered and restored on subsequent visits.
  - Density defaults to Comfortable, with Compact available.
  - Density changes spacing and information density without changing the semantic design tokens or hiding critical information.
- Density is one global preference across the workspace: Comfortable by default, Compact optional. Phase 2 does not define per-panel overrides; each surface may still adapt spacing/layout to viewport constraints while respecting the selected density.
- Design versions are immutable snapshots or versioned revisions.
- Prototype preview is labelled as design/prototype, not a real deployed app.
- An external Figma/Penpot connection displays its actual permission state and available actions.
- If external authoring is unavailable, the operator can continue in the native design workspace.

## 17. Design generation and review states

| Stage | UI status | Operator action |
|---|---|---|
| Brief | Draft / Ready for review | Confirm product goals and constraints |
| UX structure | Generating / Ready | Review screen inventory and journey coverage |
| Wireframes | Draft / Ready | Request layout changes |
| Visual system | Draft / Ready | Confirm theme, typography and token direction |
| Screens | Generating / Needs review | Inspect screen composition and responsive variants |
| Prototype | Building / Ready | Test main journeys |
| Critique | Running / Findings ready | Review evidence-backed findings |
| Approval | Awaiting owner / Approved / Rejected | Accept or request revisions |
| Handoff | Ready / Incomplete | Confirm implementation package |

