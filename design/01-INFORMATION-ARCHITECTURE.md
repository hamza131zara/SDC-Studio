# SDC Studio — Information Architecture

**Status:** Design draft  
**Work item:** P2-01  
**Purpose:** Define page hierarchy, navigation, project context, and workspace structure before visual design and implementation.

## 1. UX goals

1. Make starting a conversation or project immediately obvious.
2. Keep the chat as the primary command and reasoning interface.
3. Make the current project/repository context visible at all times.
4. Let the operator inspect code changes and execution evidence without losing the conversation.
5. Make agent activity understandable: what it is doing, why, and what changed.
6. Keep modes, model selection, and execution autonomy distinct.
7. Reduce navigation friction while avoiding an overcrowded IDE layout.
8. Support multiple projects without mixing their conversations, repositories, files, or task histories.

## 2. Primary navigation model

### Global sidebar

The left sidebar is persistent on desktop and collapsible on narrower screens.

**Top region**
- SDC Studio logo/name.
- New Chat action.
- New Project action.

**Workspace region**
- Recent conversations.
- Projects list with project name and status indicator.
- Optional pinned projects.
- Search/filter across conversations and projects.

**Bottom region**
- Integrations (GitHub and configured providers).
- Settings.
- Help/About.
- Operator account/session indicator.

The sidebar must not become a second project file explorer. Repository files belong in the project workspace.

### Main content region

The center area changes according to route/context:
- Home/new conversation.
- Conversation.
- Project overview.
- Project workspace.
- Settings/integrations.
- Task/report detail.

### Context/workspace panel

In a project conversation, the operator can reveal a right-side contextual panel containing:
- Project summary.
- Connected repository, branch, and baseline.
- Current task status.
- Active execution mode and autonomy.
- Files changed.
- Recent tool activity and evidence shortcuts.

The panel is optional/collapsible; the chat remains readable.

## 3. Page hierarchy

    SDC Studio
    ├── Home / New Chat
    │   ├── Prompt composer
    │   ├── Mode selector
    │   ├── Model selector
    │   ├── Autonomy selector
    │   ├── Attachment controls
    │   └── Project/repository context selector
    ├── Conversations
    │   └── Conversation detail
    │       ├── Message stream
    │       ├── Task/activity timeline
    │       ├── Composer
    │       └── Context panel
    ├── Projects
    │   └── Project overview
    │       ├── Project summary
    │       ├── Repository connection
    │       ├── Conversations/tasks
    │       └── Project workspace
    │           ├── Chat
    │           ├── Files and changes
    │           ├── Preview
    │           ├── Terminal
    │           ├── Tests and QA
    │           ├── Activity
    │           └── Reports
    ├── Integrations
    │   ├── GitHub
    │   └── AI Providers / Local Models
    └── Settings
        ├── General
        ├── Models and provider preferences
        ├── Execution and permissions
        ├── Project defaults
        └── Appearance

The hierarchy describes navigation destinations, not a requirement to build every page in Phase 3 simultaneously. Phase implementation follows the approved roadmap.

## 4. Home / new conversation

The home screen answers three questions:
- What do I want the AI to do?
- Which project/repository is this about?
- How much autonomy should it use?

Core elements:
1. Greeting/short instruction, not a marketing landing page.
2. Large message composer.
3. Attach file/image control.
4. Optional project/repository selector.
5. Operating mode selector.
6. Model/provider selector.
7. Ask/Guided/Auto execution selector.
8. Recent projects/conversations below or in the sidebar.

Avoid large dashboard cards that compete with the primary prompt.

## 5. Project information architecture

Every project has:
- Name and description.
- Source: blank workspace, local workspace, or connected GitHub repository.
- Repository identity and selected branch if connected.
- Conversation history.
- Task history and status.
- Current workspace revision.
- Artifacts and reports.

A project workspace has a consistent top-level project header and tabbed or segmented work surfaces. The Design surface is used to create/review design briefs, flows, wireframes, screens, tokens, components and prototypes; it is distinct from Preview, which shows a running implementation.

### Recommended project workspace navigation

- **Design:** Design brief, IA, user journeys, wireframes, screens, tokens, components and prototype review.
- **Chat:** Primary task conversation and agent interaction.
- **Files:** Repository tree and file viewer.
- **Changes:** Diff, changed files, commits, and PR references.
- **Preview:** Running application preview.
- **Terminal:** Isolated workspace command output.
- **QA:** Test runs, screenshots, browser console/network diagnostics.
- **Activity:** Agent actions and tool execution history.
- **Reports:** Final task reports and downloadable artifacts.

The tabs should be permission/state-aware. For example, Preview can show a helpful empty state until an app is running; it must not imply that a preview exists.

## 6. Project context visibility

A compact project header should show:
- Project name.
- Repository name/owner when connected.
- Current branch.
- Connection/runner status.
- Current task status.

Avoid showing raw tokens, private key details, environment variable values, or full sensitive filesystem paths.

## 7. Navigation rules

- Selecting a project opens its overview, not an unrelated global conversation.
- Selecting a recent conversation restores its project and task context.
- Switching projects must not preserve stale file/terminal/preview state from the previous project.
- A task report can be opened independently and linked back to its conversation.
- A deep link to a task or report must verify project access.
- Sidebar collapse state may persist as a UI preference; sensitive project data must not be stored in insecure browser storage.

## 8. Empty and first-use states

- No projects: explain Create Project and Connect Repository.
- No conversations: show the composer and recent project entry points.
- No GitHub installation: explain how to connect GitHub and select repositories.
- No AI providers: explain that a model provider/local model must be configured before agent execution.
- No running preview: explain how to start a development server through the task workflow.
- No test runs: show the test entry point and state that no QA has run yet.
- No reports: show that reports are generated after tasks execute.

## 9. Desktop and responsive information hierarchy

Desktop:
- Sidebar visible.
- Main chat/workspace occupies primary width.
- Context panel can open on the right.
- IDE surfaces may use split panes.

Tablet:
- Sidebar collapses to a drawer.
- Context panel becomes a drawer.
- Workspace tabs remain visible or horizontally scrollable.
- Avoid three simultaneously narrow panes.

Mobile:
- Chat and report reading are primary.
- Project context and navigation are drawers.
- Code/preview/terminal use dedicated full-screen tabs rather than compressed columns.
- Approval requests remain fully readable and actionable.

## 10. Design workspace entry point

The Design surface must make the stages of design visible: Brief, UX Structure, Wireframes, Visual System, Screens, Prototype, Review and Handoff. It must show which artifacts are drafts, which are approved, and which require owner input. It should support native SDC design artifacts and optional Figma/Penpot connections without requiring an external design account.

## 11. Information architecture acceptance

- The route/page hierarchy is clear and non-duplicative.
- New Chat and New Project are discoverable.
- Project and repository context remain visible in the appropriate workspace.
- Chat, code changes, preview, terminal, QA, activity, and reports have clear destinations.
- Settings and integrations do not compete with primary task entry.
- Empty/loading/error states are accounted for.
