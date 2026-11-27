# SDC Studio — Screen Wireframe Specification

**Status:** Design draft  
**Work items:** P2-03, P2-04, P2-05, P2-06, P2-07, P2-09  
**Purpose:** Define low-fidelity page composition and component placement before high-fidelity visual design or implementation.

The wireframes below are structural descriptions. They intentionally avoid committing to final brand colors, logo artwork, or pixel dimensions until the design system and owner review are accepted.

## 1. Shared desktop application shell

    ┌────────────────────────────────────────────────────────────────────────────┐
    │ SDC Studio                                    Project / Model / Profile     │
    ├────────────────┬──────────────────────────────────────────┬────────────────┤
    │ LEFT SIDEBAR   │ MAIN WORK AREA                            │ CONTEXT PANEL  │
    │                │                                           │ (collapsible)  │
    │ + New Chat     │ Current page / conversation               │ Project        │
    │ + New Project  │                                           │ Repository     │
    │                │                                           │ Branch         │
    │ Recent chats   │ Main content                              │ Active task    │
    │                │                                           │ Files changed  │
    │ Projects       │                                           │ Recent actions │
    │  - Project A   │                                           │ Artifacts      │
    │  - Project B   │                                           │                │
    │                │                                           │                │
    │ Integrations   │                                           │                │
    │ Settings       │                                           │                │
    │ Account        │                                           │                │
    └────────────────┴──────────────────────────────────────────┴────────────────┘

Shared behavior:
- Sidebar can collapse without losing the active route.
- Context panel opens/closes independently.
- Header shows current page/project and active connection status.
- Main content owns vertical scrolling; panes should not cause accidental whole-page scroll where independent scrolling is needed.
- At mobile widths, sidebar and context panel become drawers.

## 2. Screen A — Home / New Chat

    ┌────────────────┬───────────────────────────────────────────────┐
    │ SDC Studio     │ New conversation                              │
    │ + New Chat     │                                               │
    │ + New Project  │       How can SDC Studio help today?           │
    │                │                                               │
    │ Recent         │       [Describe a task or ask a question...]   │
    │ Projects       │                                               │
    │                │       [Attach] [Image] [Project/Repo]          │
    │ Integrations   │       [Mode: Auto] [Model: Auto] [Auto] [Send] │
    │ Settings       │                                               │
    │                │ Recent projects / suggested recent chats      │
    └────────────────┴───────────────────────────────────────────────┘

Required elements:
- Prominent multi-line composer.
- Submit action enabled only when the request is valid.
- Attachment control with accepted-file and size guidance.
- Optional project/repository context.
- Separate selectors for mode, model, and autonomy.
- Recent project/conversation entry points.

Composer states:
- Empty.
- Typing.
- Attachment added.
- Uploading.
- Unsupported file.
- Oversized file.
- Submitting.
- Streaming response.
- Provider unavailable/quota exceeded.
- Retryable connection interruption.

Do not overload the home screen with analytics cards or marketing content.

## 3. Screen B — New Project dialog

    ┌──────────────────────────────────────────────┐
    │ Create a project                         [X] │
    │                                              │
    │ Project name                                 │
    │ [________________________________________]   │
    │ Description (optional)                       │
    │ [________________________________________]   │
    │                                              │
    │ Start with                                   │
    │ (•) Blank workspace                          │
    │ ( ) Connect a GitHub repository              │
    │                                              │
    │                [Cancel] [Create project]     │
    └──────────────────────────────────────────────┘

Rules:
- Project name is required.
- Description is optional.
- Blank workspace must be available without GitHub.
- If GitHub option is selected, continue to the dedicated repository connection flow.
- Do not imply that a repository is created on GitHub unless that action is separately implemented and authorized.

## 4. Screen C — Conversation

    ┌────────────────┬──────────────────────────────────┬───────────────┐
    │ Sidebar        │ Conversation title                │ Context       │
    │                │ Project: Project A  Branch: task-x │ Panel         │
    │ Recent chats   │                                  │               │
    │                │ User message                      │ Project       │
    │ Projects       │                                  │ Repo          │
    │                │ Assistant response                │ Branch        │
    │                │                                  │               │
    │                │ ┌──────────────────────────────┐ │ Active task   │
    │                │ │ Task plan / activity         │ │ Files changed │
    │                │ │ 1. Inspect project           │ │ Artifacts     │
    │                │ │ 2. Plan changes              │ │               │
    │                │ │ 3. Implement                 │ │               │
    │                │ │ 4. Verify                    │ │               │
    │                │ └──────────────────────────────┘ │               │
    │                │                                  │               │
    │                │ [Attach] [Prompt composer...]    │               │
    │                │ [Mode] [Model] [Autonomy] [Send] │               │
    └────────────────┴──────────────────────────────────┴───────────────┘

Required behavior:
- Messages are visually distinct from tool/activity events.
- Assistant messages can contain plan summaries, citations, code, and report sections.
- Activity cards can expand to show operation details and output.
- Long-running task status remains visible while scrolling.
- The composer remains available when the task permits another message.
- User can stop/cancel a running task where the runtime supports safe cancellation.
- A context panel may show project/repository state without duplicating the main conversation.

## 5. Screen D — Project Overview

    ┌───────────────────────────────────────────────────────────────┐
    │ Project A                          [Connect repo] [New task]  │
    │ Description / project summary                                │
    ├───────────────────────────────────────────────────────────────┤
    │ Repository: owner/repo   Branch: main   Status: Connected    │
    ├───────────────────────────────────────────────────────────────┤
    │ [Chat] [Files] [Changes] [Preview] [Terminal] [QA] [Activity] │
    │ [Reports]                                                     │
    ├───────────────────────────────────────────────────────────────┤
    │                                                               │
    │ Project summary                                               │
    │ Recent conversations and tasks                                │
    │ Latest task status                                            │
    │ Repository health / last verified baseline                    │
    │ Recent reports and screenshots                                │
    │                                                               │
    └───────────────────────────────────────────────────────────────┘

Required content:
- Project identity and description.
- Repository connection state.
- Current/default branch and baseline where known.
- Recent tasks and status.
- Quick access to primary project surfaces.
- Clear indication if no repository is connected.

## 6. Screen E — Project Files

    ┌───────────────────────────────────────────────────────────────┐
    │ Project A  / Files                                             │
    ├───────────────────┬───────────────────────────────────────────┤
    │ File tree         │ File viewer                                │
    │                   │ path/to/file.ts                            │
    │ src/              │ ─────────────────────────────────────────  │
    │  app/             │                                           │
    │  components/      │ Code with line numbers                     │
    │ package.json      │                                           │
    │ README.md         │                                           │
    │                   │                                           │
    └───────────────────┴───────────────────────────────────────────┘

Rules:
- Show relative project paths.
- Do not expose host absolute paths or secret values.
- Mark files modified by the active task.
- Distinguish tracked, untracked, ignored, and unavailable files where known.
- Editing behavior is available only through authorized workspace tools and must create an auditable diff.
- Sensitive files can be masked or excluded according to project settings.

## 7. Screen F — Changes / Diff Review

    ┌───────────────────────────────────────────────────────────────┐
    │ Project A / Changes          Branch: task/feature-x            │
    ├──────────────────────┬────────────────────────────────────────┤
    │ Changed files        │ Selected file diff                     │
    │                      │                                        │
    │ + NewComponent.tsx   │ - old code                              │
    │ M page.tsx           │ + new code                              │
    │ M package.json       │                                        │
    │                      │                                        │
    │ Summary:             │ [Unified] [Split]                       │
    │ 2 modified           │                                        │
    │ 1 added              │                                        │
    │ 0 deleted            │                                        │
    ├──────────────────────┴────────────────────────────────────────┤
    │ [Open task activity] [Commit/PR status] [Request revision]    │
    └───────────────────────────────────────────────────────────────┘

Required elements:
- Added/modified/deleted/renamed counts.
- File list and per-file diff.
- Unified and split diff modes if supported by chosen editor component.
- Task/branch/baseline references.
- Related test/evidence links.
- Clear state if no changes exist.
- No automatic merge button in the ordinary task diff surface.

## 8. Screen G — Preview

    ┌───────────────────────────────────────────────────────────────┐
    │ Project A / Preview          [Desktop] [Tablet] [Mobile]      │
    │ Preview: Running  http://preview-session/... [Open separately]│
    ├───────────────────────────────────────────────────────────────┤
    │                                                               │
    │                 EMBEDDED APPLICATION PREVIEW                  │
    │                                                               │
    │                                                               │
    └───────────────────────────────────────────────────────────────┘
    │ [Reload] [Capture screenshot] [Run browser checks] [Stop]     │

States:
- Not started.
- Starting/readiness check.
- Running.
- Build failed.
- Server stopped.
- Preview expired.
- Access denied.
- Runner unavailable.

The UI must never display a fake/placeholder URL as a working preview. Preview origin and revision should be visible.

## 9. Screen H — Terminal

    ┌───────────────────────────────────────────────────────────────┐
    │ Project A / Terminal      Workspace: task-123   [Clear view]  │
    ├───────────────────────────────────────────────────────────────┤
    │ $ npm run build                                               │
    │ > build                                                       │
    │ ...                                                           │
    │ ✓ Build completed                                             │
    │                                                               │
    │ Command: npm run build   Exit code: 0   Duration: 24s         │
    └───────────────────────────────────────────────────────────────┘

Terminal presentation:
- Readable command and output blocks.
- Working directory relative to workspace.
- Running/exit status and duration.
- Expandable stdout/stderr.
- Redacted secrets.
- Clear indicator that commands run in an isolated project workspace.
- Command execution input is enabled only according to configured permissions/autonomy.
- Destructive/high-impact commands require an approval explanation.

## 10. Screen I — QA and Test Evidence

    ┌───────────────────────────────────────────────────────────────┐
    │ Project A / QA                         [Run test profile]     │
    ├───────────────────────────────────────────────────────────────┤
    │ Latest run: Completed with issues   Revision: abc1234         │
    │                                                               │
    │ Unit tests       Passed / Failed / Skipped                    │
    │ Integration      Passed / Failed / Not run                    │
    │ E2E              Passed / Failed / Not configured             │
    │ Browser          Completed / Failed                            │
    │ Accessibility    Completed / Not run                           │
    │ Security         Completed / Not configured                    │
    ├───────────────────────────────────────────────────────────────┤
    │ Screenshots | Console | Network | Traces | Test output        │
    │ [thumbnail] [thumbnail]                                       │
    │                                                               │
    │ Defects and repair history                                    │
    └───────────────────────────────────────────────────────────────┘

Required distinctions:
- Passed, failed, skipped, blocked, not run, and not configured.
- Tested revision and browser/viewport.
- Screenshots linked to a test run.
- Console/page errors separate from warnings.
- Failed network requests separate from non-2xx responses.
- Repairs and retests retain their history.

## 11. Screen J — Agent Activity

    ┌───────────────────────────────────────────────────────────────┐
    │ Task Activity                         Status: Running         │
    ├───────────────────────────────────────────────────────────────┤
    │ 09:41  Repository scan                  Completed             │
    │ 09:42  Read package.json                Completed             │
    │ 09:43  Plan implementation              Completed             │
    │ 09:44  Create task branch               Completed             │
    │ 09:45  Update component                  Running               │
    │                                                               │
    │ [Expand operation]                                            │
    │ Tool: filesystem.write                                        │
    │ Target: src/components/...                                    │
    │ Reason: implement acceptance criterion FR-...                │
    │ Result: pending                                               │
    └───────────────────────────────────────────────────────────────┘

Activity event fields:
- Timestamp.
- Agent role.
- Action/tool name.
- Reason/related task step.
- Target resource (redacted/scoped).
- Status.
- Result summary.
- Evidence/diff link where applicable.

Do not display chain-of-thought or private model reasoning. Show concise action explanations, plans, tool events, and outcomes.

## 12. Screen K — Integrations

Separate connection cards/sections:
- GitHub App: connected/disconnected, installation owner, selected repository scope, granted permissions summary.
- AI Providers: provider name, configured state, enabled models, test status, data-term status and quota state where available.
- Local Model Runtime: configured/unavailable, runtime endpoint label, model list and health status.

Never display full credentials. Use masked credential status and replace/revoke controls.

## 13. Screen L — Settings

Settings sections:
- General: product preferences and default project behavior.
- Appearance: theme and density preferences.
- Models and Providers: configured provider/model defaults.
- Execution: Ask/Guided/Auto defaults, tool categories and resource profile.
- Permissions: project-scoped read/write/terminal/browser/deployment access.
- GitHub: installation/repository scope and access state.

Settings must clearly distinguish a default preference from a hard security policy or current external provider limitation.

## 14. Screen M — Approval request

    ┌────────────────────────────────────────────────┐
    │ Approval required                              │
    │ Production deployment                           │
    │                                                 │
    │ Project: Project A                              │
    │ Target: production / selected provider          │
    │ Reason: publish approved application build      │
    │ Impact: externally visible application change   │
    │ Reversible: provider rollback supported/unknown │
    │                                                 │
    │ [View deployment plan and diff]                 │
    │                                                 │
    │ [Reject]                       [Approve action] │
    └────────────────────────────────────────────────┘

Approval copy must be specific, plain language, and operation-bound. Avoid ambiguous generic buttons such as "Allow everything".

## 15. Screen N — Task Report

    ┌───────────────────────────────────────────────────────────────┐
    │ Task Report                              Completed with issues│
    ├───────────────────────────────────────────────────────────────┤
    │ Requested outcome                                             │
    │ Summary of work                                                │
    │                                                               │
    │ Files changed / branch / commit / PR                          │
    │ Tests actually executed                                        │
    │ Screenshots and browser evidence                               │
    │ Warnings, failed checks, untested areas                       │
    │                                                               │
    │ Recommended next action                                       │
    └───────────────────────────────────────────────────────────────┘

Report sections must link to underlying files, tool events, test runs, and artifacts where available.

## 16. Screen acceptance

The screen set is structurally complete when:
- Primary operator journeys have a dedicated screen or clear component.
- Project context is not lost between chat and IDE surfaces.
- Every asynchronous operation has visible status and failure behavior.
- Approval screens show target, impact, and exact action.
- No screen claims a connection, test, preview, or deployment exists without evidence.
- Responsive equivalents are defined in the interaction specification.


## 17. Screen O — Design Workspace

    ┌────────────────────────────────────────────────────────────────────┐
    │ Project A / Design             Version: Draft v3     [Review] [Save]│
    ├────────────────────┬───────────────────────────────┬───────────────┤
    │ DESIGN STAGES      │ CANVAS / SCREEN                │ DESIGN BRIEF  │
    │                    │                                │               │
    │ Brief              │  [Desktop] [Tablet] [Mobile]   │ Product       │
    │ UX Structure       │                                │ Target users  │
    │ Wireframes         │     Selected editable screen   │ Constraints   │
    │ Visual System      │                                │ Requirements  │
    │ Screens            │                                │               │
    │ Prototype          │                                │ Tokens        │
    │ Review             │                                │ Components    │
    │ Handoff            │                                │ Findings      │
    │                    │                                │               │
    ├────────────────────┴───────────────────────────────┴───────────────┤
    │ [Ask Designer] [Compare versions] [Preview prototype] [Export]     │
    └────────────────────────────────────────────────────────────────────┘

Required elements:
- Visible design stages and per-stage status.
- Editable screen/frame canvas or rendered native design preview.
- Screen and viewport selection.
- Brief, requirement and reference panel.
- Component/token inspector.
- Agent activity and design review findings.
- Version history and comparison.
- Prototype preview and handoff controls.
- External tool connection state (Figma/Penpot) without blocking native design work.

States:
- No design brief yet.
- Brief ready for review.
- UX structure in progress.
- Wireframe draft.
- Visual system draft.
- Screens generating.
- Prototype ready.
- Review findings present.
- Awaiting owner approval.
- Approved design.
- External design tool unavailable/permission denied.
- Design revision in progress.

The screen must distinguish design preview from a running application preview. A design screen cannot claim to be tested code.

## 18. Screen acceptance extension

- Design Workspace stages are visible and understandable.
- The operator can inspect/edit the brief and generated design artifacts.
- Prototype status is distinguishable from a live running application.
- Design versions and approval state are visible.
- Figma/Penpot connectivity is optional; the native design experience remains usable.
