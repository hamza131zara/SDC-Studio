# SDC Studio — Phase 2 Design Acceptance Criteria

**Status:** Accepted with owner-approved exception  
**Phase:** 2 — UI/UX Design  
**Purpose:** Define objective completion conditions before Phase 3 application implementation may begin.

## 1. Phase 2 exit criteria

Phase 2 is accepted when:

- [ ] Information architecture and page hierarchy are approved.
- [ ] New chat and new project flows are specified.
- [ ] Existing GitHub repository connection flow is specified, including selected repository scope and read-only discovery state.
- [ ] Project Settings contains a dedicated Integrations & APIs management area for multiple supported platforms/tools.
- [ ] Integration catalog is searchable/category-based and exposes adapter-supported authentication methods, operations, required scopes and data handling.
- [ ] Project integrations support protected credential setup, encrypted-at-rest server-side storage, project binding, safe Test Connection/Check Status where supported, and truthful unsupported/untested states.
- [ ] Potentially billable sample API/model requests are separate from safe connection checks and require explicit operator initiation.
- [ ] Project integration cards expose actual status, last-check evidence, available usage/quota information where reported, and recovery actions.
- [ ] Agent tool calls are limited to configured/enabled project integrations and their permitted adapter operations; activity is auditable with secrets redacted.
- [ ] All eight operating modes are represented in the interface.
- [ ] Ask, Guided, and Auto autonomy settings are separate from mode and model selectors.
- [ ] Core user journeys include success, empty, loading, error, blocked, and approval states.
- [ ] Main application shell layout is specified for desktop, tablet, and mobile.
- [ ] A dedicated Design workspace is specified for brief, UX structure, wireframes, visual system, screens, prototype, review and handoff.
- [ ] Chat experience includes composer, attachments, streaming, task plan, activity, stop/error states, and context.
- [ ] Project workspace includes Chat, Files, Changes, Preview, Terminal, QA, Activity, and Reports surfaces.
- [ ] Execution transparency exposes tool activity and operation outcomes without exposing private model reasoning or secrets.
- [ ] Approval interaction shows exact target, action, impact, and reversibility.
- [ ] Design system defines typography, spacing, semantic color tokens, surfaces, icons, component inventory, and states.
- [x] Theme preference is owner-confirmed: both Light and Dark; follow OS preference on first visit and remember the last manual choice.
- [x] Density preference is owner-confirmed: Comfortable default; Compact optional; one predictable global setting without per-panel overrides.
- [ ] Brand accent/logo direction is reviewed by the owner.
- [ ] Responsive behavior and accessibility requirements are documented.
- [ ] Clickable interaction prototype or an equivalent owner-approved interactive design handoff covers the primary journeys.
- [ ] AI design workflow and external-tool integration architecture are documented, including native design artifacts, Figma/Penpot adapters, vision critique, versioning and handoff.
- [ ] Project integrations settings cover adding/selecting multiple tool platforms, protected authentication, project scope, test/status support, usage visibility and safe removal.
- [ ] All design deliverables are linked from the design index.
- [ ] Owner approves the Phase 2 design package.

## 2. Screen coverage matrix

| Screen/surface | Wireframe | Journeys | Responsive | States |
|---|---:|---:|---:|---:|
| Home / New Chat | Yes | A | Yes | Yes |
| New Project | Yes | B | Yes | Yes |
| Conversation | Yes | A, D, H | Yes | Yes |
| Project Overview | Yes | B, C | Yes | Yes |
| Files | Yes | D, F | Yes | Yes |
| Changes / Diff | Yes | F | Yes | Yes |
| Preview | Yes | G | Yes | Yes |
| Terminal | Yes | D, G | Yes | Yes |
| QA | Yes | G, H | Yes | Yes |
| Activity | Yes | D, E | Yes | Yes |
| Project Settings / Integrations & APIs | Yes | C, I, M | Yes | Yes |
| Settings | Yes | I | Yes | Yes |
| Approval | Yes | E, J | Yes | Yes |
| Task Report | Yes | H | Yes | Yes |

## 3. Product requirement alignment

Design must reflect:
- FR-CHAT-01 to FR-CHAT-03: conversations, streaming and attachments.
- FR-PROJ-01 to FR-PROJ-04: projects and repository context.
- FR-MODE-01 and FR-MODE-02: modes and model selection independence.
- FR-AUTO-01 to FR-AUTO-04: autonomy and approval interaction.
- FR-AI-01 to FR-AI-06: provider/model settings and availability states.
- FR-AGENT-01 to FR-AGENT-06: plans, activity, task status and tool outcomes.
- FR-REPO-01 to FR-REPO-05: repository discovery, branch, diff and PR states.
- FR-QA-01 to FR-QA-07: preview, tests, diagnostics and evidence.
- FR-REPORT-01: task report.
- FR-DEPLOY-01 and FR-DEPLOY-02: deployment compatibility and approval states.

The UI must not imply that later-phase backend functionality already exists.

## 4. Visual quality criteria

- Consistent spacing, typography, borders, surfaces and icon usage.
- Strong hierarchy and readable information density.
- Status labels use text and semantics, not color alone.
- Code and terminal content remain legible.
- No excessive nested cards, decorative gradients, or distracting motion.
- Clear hierarchy between global navigation, project navigation, task activity and conversation.
- Dark theme is coherent; light theme is also specified if retained in the accepted scope.
- Branding tokens can be changed centrally.

## 5. Responsive criteria

- Wide desktop supports sidebar, primary workspace and optional context panel.
- Tablet collapses secondary navigation into drawers and avoids crowded three-pane layouts.
- Mobile prioritizes chat, reports, task status and approval actions; IDE tools open as dedicated surfaces.
- No essential action is hidden solely because a device has a narrow width.
- Dialogs, file upload, tabs, and action menus remain usable at mobile sizes.

## 6. Accessibility criteria

- Target WCAG 2.2 AA for primary workflows.
- Keyboard access to all controls.
- Visible focus states.
- Semantic headings and landmarks.
- Accessible names for icon-only buttons.
- Dialog focus management.
- Sufficient text/control contrast.
- Non-color status distinction.
- Reduced-motion behavior.
- Readable error messages and input labels.

## 7. Prototype acceptance

The prototype must let the owner navigate the main happy paths:
- Start a new chat.
- Create a project.
- Connect a repository conceptually.
- Select mode, model and autonomy independently.
- Inspect plan/activity.
- Review an approval.
- Open Files/Changes/Preview/QA/Activity/Reports.
- Inspect a final report.

Prototype content may be simulated, but all simulated states must be clearly understood as design examples rather than live system execution.

## 8. Phase 2 exclusions

Phase 2 does not include:
- Next.js application source code.
- Backend/API implementation.
- Database schema migrations.
- Provider API credentials or live model integrations.
- Product GitHub App registration/integration.
- Docker runner or command execution.
- Live project preview implementation.
- Playwright test engine implementation.
- Hosting/deployment implementation.

These are Phase 3 onward work items.

## 9. Design-agent architecture acceptance

- [ ] Design workflow is staged from brief to UX structure, wireframe, visual system, editable screens, prototype, critique, owner approval and handoff.
- [ ] Design outputs are editable semantic artifacts; a full-screen screenshot is not accepted as the source design.
- [ ] Figma/Penpot are optional adapters, not mandatory dependencies.
- [ ] A native SDC design representation/preview is defined as the fallback when external authoring is unavailable.
- [ ] Provider/model visual capability is detected; vision critique is never falsely claimed for text-only models.
- [ ] Design quality findings reference specific screens/components and evidence.
- [ ] Approved design version and handoff are traceable to product requirements.

## 10. Phase approval record

**Owner acceptance:** 2026-11-27  
**Accepted scope:** Written information architecture, user journeys, wireframes, visual design system, interaction/responsive specification, project integrations settings requirements, and AI design workflow/integration architecture.  
**Branch / PR:** phase-2/ui-ux-design / PR #3  
**Merge commit:** 07f9e0313bebb5ad604c9fd51f031a303edfc647  
**Theme and density:** Both Light and Dark; follow OS preference initially and remember manual choice; Comfortable default with Compact optional.

**Owner-approved exception:** The editable clickable prototype or equivalent interactive design handoff (P2-09) was waived as a Phase 2 exit prerequisite. It was not created and remains deferred. This is an explicit exception, not a completed prototype acceptance. Final SDC brand accent/logo and workspace navigation polish are deferred for implementation refinement.

**Phase decision:** Phase 2 written design package accepted with the above exception. Phase 3 — Application Shell is authorized to begin using the written design documents as the implementation reference.
