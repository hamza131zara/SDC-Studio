# SDC Studio — Product Design Prototype Brief

**Phase:** 2 — UI/UX Design  
**Work items:** P2-09, P2-10  
**Status:** Ready to use with the connected Product Design app; prototype not yet generated/approved  
**Source of truth:** Phase 0 Foundation, Phase 1 documentation, and the Phase 2 design package

## 1. Design assignment

Create a polished, professional, desktop-first product UI and interactive prototype for **SDC Studio**, a private agency engineering workspace that combines AI conversations with a complete software delivery environment.

The experience should feel like a high-quality AI product workspace combined with a developer IDE. It must be visually coherent, practical for long engineering sessions, and clear about what the AI is actually doing.

This is a design/prototype assignment only. Do not claim there is a live backend, repository connection, running code, real test execution, or deployment. Use clearly labelled sample data for prototype interactions.

## 2. Product context

SDC Studio is intended to help an agency owner build and maintain websites, web apps, mobile apps, DApps, smart contracts, APIs, LLM applications, and AI agents.

The platform's planned lifecycle includes:
- Ask and research.
- Product planning and requirements.
- Documentation and architecture.
- UI/UX design and prototyping.
- Development and code modification.
- Testing, QA, debugging and regression.
- Browser preview, screenshots, console/network diagnostics.
- Deployment guidance and authorized publishing.
- Maintenance and progress reporting.

The operator works primarily through natural-language conversations, selects a project/repository, chooses an operating mode and model, and controls execution autonomy.

## 3. Primary design objective

Make the system easy to direct like a chat assistant while providing the transparency and working surfaces of a serious development tool.

The interface must quickly answer:
1. Which project/repository am I working on?
2. Which task is active?
3. What has the agent done so far?
4. Which files changed?
5. Which tests actually ran?
6. Are there approvals or blockers?
7. What is the next action?

Do not turn the product into a generic analytics dashboard. The chat and active project workspace must remain central.

## 4. Required pages/screens

Create the following as separate editable frames/screens, not one flattened image.

### Screen 1 — Home / New Chat
- Left navigation with SDC Studio identity, New Chat, New Project, recent chats, projects, integrations and settings.
- Centered chat composer with file/image attachment.
- Separate selectors for Mode, Model and Autonomy.
- Recent projects/conversations as secondary content.
- Primary focus should be the prompt composer.

### Screen 2 — Project Workspace / Chat
- Project and repository identity in the header.
- Current branch and connection status.
- Chat message stream.
- Task plan and progress timeline.
- Expandable agent activity events.
- Composer at the bottom.
- Optional contextual right panel with project summary, files changed, repository details and artifacts.

### Screen 3 — Design Studio
- Design stages: Brief, UX Structure, Wireframes, Visual System, Screens, Prototype, Review and Handoff.
- Main editable screen canvas/preview.
- Desktop/tablet/mobile viewport selector.
- Right inspector with requirements, design tokens, components and review findings.
- Visible version status and owner approval state.

### Screen 4 — Changes / Diff
- Changed file summary (added, modified, deleted, renamed).
- File list on the left and readable code diff on the right.
- Link to associated task, tool event, test, branch and PR.
- Clear empty state when no changes exist.

### Screen 5 — Preview and QA
- Embedded application preview area, visibly labelled as a prototype/sample preview in this design assignment.
- Desktop/tablet/mobile viewport controls.
- QA summary showing passed, failed, skipped, blocked and not-run—not just green success counts.
- Screenshot thumbnails and sections for Console, Network, Traces and Test Output.
- Example warning or failed request to demonstrate the diagnostics UI.

### Screen 6 — Activity and Approval
- Chronological activity list with agent role, action, tool name, purpose, target, status and result.
- Approval card with exact action, project/target, impact, reversibility, and Approve/Reject.
- Activity must show concise action explanations, not private model chain-of-thought.

### Screen 7 — Task Report
- Completion state such as Completed with Issues.
- Requested outcome and work summary.
- Files changed and branch/PR reference.
- Actual test results and browser evidence.
- Warnings, untested areas, unresolved issues and direct next action.

### Screen 8 — Project Settings / Integrations & APIs

Show a clearly project-scoped settings page, opened from the active project's header/menu. Include:

- Project identity and settings navigation.
- Searchable integration catalog grouped by AI/model providers, development/design tools, source control, testing/browser, hosting/deployment, research/search and other supported tools.
- Platform detail panel explaining purpose, supported operations, accepted authentication methods, required scopes, data sent, status-test availability and usage/cost notes.
- Add/configure flow with masked API-key field where supported; show OAuth/token/local endpoint alternatives only where appropriate for the selected adapter.
- Configured project integration cards with platform, configured/verified/disabled status, capabilities/scopes, last checked time, and provider-reported quota/usage where available.
- Actions for Configure, Test Connection, Check Status, Enable/Disable for this project, Replace Credential, Reauthorize and Remove Project Binding.
- Clear “Configured — not verified” state after a key is saved but no successful test has occurred.
- Show “Test/status check unsupported” where the adapter cannot safely verify.
- A potentially billable sample API/model request must be a separate explicit action with a cost/usage warning.
- Never display a real API key or invite the operator to paste a real secret into the prototype. Use clearly fictional masked sample values and simulated statuses only.

### Screen 9 — Global Settings & Appearance
- Account/session and application-wide defaults.
- Both Light and Dark themes; OS preference before manual selection, then remember last manual choice.
- Comfortable density as default, Compact as optional global setting.
- No project API key or project tool connection form here; those belong in Screen 8.

## 5. Visual direction

Create a refined developer-focused interface:
- Neutral graphite/charcoal surfaces in Dark theme.
- Clean white/soft-neutral surfaces in Light theme.
- Restrained accent color; use semantic colors only for meaningful statuses.
- Clear typography and strong content hierarchy.
- Subtle borders and surface separation, not excessive cards or shadows.
- Consistent iconography.
- Good spacing and readable code/terminal output.
- Avoid excessive gradients, glass effects, neon decoration, giant empty hero areas, and marketing-site styling.

The final SDC brand accent/logo is not yet approved. Use a neutral placeholder token and label it as provisional; do not invent an official brand identity.

## 6. Owner-selected global preferences

### Theme
- Provide both Dark and Light modes.
- On the first visit, follow the operating-system theme preference.
- Once the operator manually chooses Dark or Light, remember that last selection and restore it on subsequent visits.
- Demonstrate both themes as variants in the design system.

### Density
- Comfortable is the default.
- Compact is an optional preference for information-dense work surfaces.
- Comfortable: more spacing in chat, reports, activity, settings and file rows.
- Compact: tighter row/section spacing to show more files, terminal output, activity and test records.
- Use one global density preference; do not add independent per-panel density settings in the prototype.
- Compact must not shrink text below accessible sizes or remove labels/status information.
- Show the Comfortable default plus one useful Compact example (such as a file or activity panel), without duplicating every screen.

## 7. Layout and responsive behavior

### Desktop baseline
- Design the main application at 1440px wide.
- Persistent or collapsible left sidebar.
- Flexible main workspace.
- Optional right context/inspector panel.
- Use Comfortable density for the default composition.
- Avoid three narrow columns; context panels must be collapsible.

### Tablet
- Sidebars become drawers or compact rails.
- Workspace tools use one primary surface at a time.
- Tabs can scroll horizontally.

### Mobile
- Design representative mobile screens at 390px wide.
- Chat, project status, reports and approval are primary.
- Files/diffs/terminal/preview open as dedicated views, not squeezed columns.
- Important approval actions remain readable and accessible.

## 8. Interaction prototype requirements

Create prototype links for these journeys:
1. Start a new chat.
2. Create a blank project.
3. Open a project and switch Chat / Design / Changes / Preview / QA / Activity / Reports.
4. Expand a task plan and an activity event.
5. Open a changed file diff.
6. Open Preview and switch viewport.
7. Open QA diagnostics and inspect a failed request.
8. Review and reject an approval request, then show the task as blocked or awaiting alternative.
9. Open final task report.
10. Open Global Settings and switch Dark/Light and Comfortable/Compact.
11. Open a project's Settings → Integrations & APIs, select a platform, inspect its auth options, configure a fictional masked credential, run a simulated safe connection check, and inspect Connected / Failed / Not supported status variants.
12. Show that the same platform can be configured independently for a second project, without leaking or cross-attaching credentials.

Use believable sample content such as a fictional "Agency Website Refresh" project. Clearly distinguish sample prototype data from actual repository/tool data.

## 9. Design quality requirements

- All pages use the same design system, spacing, typography and semantic status rules.
- Important actions are visually obvious without oversized buttons everywhere.
- Error, empty, loading, blocked, approval, success and completed-with-issues states are designed.
- Use accessible contrast, meaningful labels, visible focus states, and non-color status indicators.
- Keep code diffs and terminal text highly legible.
- Make the active project, current task and current mode/model/autonomy visible.
- Provide realistic but concise sample text; avoid lorem ipsum.
- Prefer editable components, frames, text and tokens over a rasterized screen.
- Create multiple visual directions only if they are materially different; recommend one and explain trade-offs.

## 10. Deliverables required from the design tool

1. Editable design file/workspace.
2. Eight screens listed above.
3. Dark and Light theme variants.
4. Comfortable density default and a Compact density example/variant.
5. Responsive representative screens for tablet/mobile.
6. Reusable components and semantic design tokens.
7. Clickable prototype covering the ten journeys.
8. Design review: requirement coverage, hierarchy, visual consistency, accessibility and responsive risks.
9. List of assumptions and unresolved decisions.
10. Handoff summary to Phase 3, but do not begin coding.

## 11. Explicit exclusions

- No production code.
- No backend or database.
- No real API key, real provider credential, real third-party API call, or billable request. All integration actions and statuses in the prototype are simulated.
- No real GitHub integration.
- No live AI model calls.
- No actual terminal execution.
- No real test execution or claim of QA success.
- No deployment.
- No changes to the approved Phase 0/Phase 1 product scope.

## 12. Review questions for owner

Before design acceptance, present:
- Recommended visual direction and why.
- Any places where the design is too dense or too spacious.
- Whether the Design Studio should be a project workspace tab or a full-screen workspace (preferred baseline: project workspace tab with an expandable full-screen canvas).
- Whether any primary user journey is missing.
- Any interaction that would be confusing or misleading.
- Any item that must be changed before developer handoff.
