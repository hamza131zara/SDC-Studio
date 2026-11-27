# SDC Studio — Phase 2 UI/UX Design Package

**Phase:** 2 — UI/UX Design  
**Status:** Accepted written design package; owner-approved P2-09 prototype waiver recorded  
**Branch:** phase-2/ui-ux-design (merged to main)  
**Review:** [PR #3 — merged](https://github.com/hamza131zara/SDC-Studio/pull/3)  
**Foundation:** Phase 0 and Phase 1 accepted and merged into main  
**Boundary:** Design artifacts only. No application source code, backend implementation, AI provider integration, GitHub App implementation, runner, preview engine, or deployment code is part of this phase.

## Design package

1. [Information Architecture](./01-INFORMATION-ARCHITECTURE.md)
2. [User Journeys](./02-USER-JOURNEYS.md)
3. [Screen Wireframes](./03-SCREEN-WIREFRAMES.md)
4. [Visual Design System](./04-DESIGN-SYSTEM.md)
5. [Interaction and Responsive Specification](./05-INTERACTION-RESPONSIVE-SPEC.md)
6. [Design Acceptance Criteria](./06-DESIGN-ACCEPTANCE.md)
7. [AI Design Workflow and Integration Architecture](./07-DESIGN-AGENT-ARCHITECTURE.md)
8. [Prototype Design Brief](./08-PROTOTYPE-DESIGN-BRIEF.md)

## Core experience

SDC Studio combines:
- A ChatGPT/Claude-style conversation experience.
- A project-centric workspace connected to blank/local/GitHub repositories.
- An IDE-style surface for files, diffs, terminal, preview, QA, evidence, and reports.
- A visible agent activity timeline and owner-controlled approval flow.

The interface must keep the conversation as the primary way to express intent while giving the operator clear visibility into project state and agent operations.

## Prototype brief

Use [08-PROTOTYPE-DESIGN-BRIEF.md](./08-PROTOTYPE-DESIGN-BRIEF.md) as the canonical prompt/brief when working with the connected Product Design app or another approved design tool.

## Owner-selected design preferences

- Theme: **Both**, with Dark and Light modes available; follow the operating-system preference on first visit and remember the operator's last manual selection.
- Density: **Comfortable by default**; offer Compact as an optional preference for information-dense workspace surfaces.

## Design status

| Work item | Status |
|---|---|
| P2-01 Information architecture | Accepted |
| P2-02 Core user journeys | Accepted |
| P2-03 Main application shell | Accepted |
| P2-04 Chat experience | Accepted |
| P2-05 Project workspace | Accepted |
| P2-06 Execution transparency | Accepted |
| P2-07 Settings and integrations | Accepted; prototype validation waived |
| P2-08 Design system | Accepted; final brand/navigation polish deferred |
| P2-09 Interaction prototype | **Waived by owner; deferred. Prototype not created.** |
| P2-10 Design review and acceptance | **Accepted with recorded exception** |
| P2-11 AI design workflow and integration architecture | Accepted; Product Design app workflow recorded |

## Confirmed owner preferences

- Both Light and Dark themes; follow the OS preference on first visit and remember the last manual selection.
- Comfortable workspace density by default; Compact is optional and uses one global setting.
- Theme and density are independent preferences.

## Remaining design assumptions for review

- Desktop-first composition because the primary workflow involves code, diffs, terminal, and preview side-by-side.
- Responsive tablet/mobile support for conversation, task status, reports, and approvals; full multi-pane development is optimized for desktop.
- Neutral visual foundation; final brand accent and logo can be aligned with SDC branding during owner review.
- One operator-focused private product; no public signup, billing, team administration, or customer tenancy screens.

These assumptions do not change the approved product scope. Owner feedback may adjust visual styling and layout within the existing product requirements.

## Design-tool access and recommendation

- **Preferred free-first editable design canvas:** Penpot, after connecting its official MCP in a supported MCP client. The cloud tier is advertised as free; exact limits must be verified during setup.
- **Figma:** currently connected, but the available teams report View seats on Starter plans; do not expect Figma Design canvas writes without Full-seat access.
- **Product Design:** use in ChatGPT Work mode for visual ideation, flow audits and prototype workflows; not a normal Chat-mode workflow or SDC Studio runtime dependency.
- **MagicPath:** connected but the account currently reports zero credits and no free plan; optional only if access changes.
- **tldraw:** suggested for system/agent diagrams; user connection is still pending.
- **Canva:** supporting graphics and brand collateral, not primary app UI source of truth.

Full tool-access notes and references are in [07-DESIGN-AGENT-ARCHITECTURE.md](./07-DESIGN-AGENT-ARCHITECTURE.md).

## Owner acceptance record — 2026-11-27

The owner accepted the written design package and authorized Phase 3. The editable clickable prototype (P2-09) was explicitly waived as a Phase 2 exit prerequisite. It has not been created and remains deferred; the written wireframes, journeys and interaction specifications are the current implementation reference. Final SDC brand accent/logo and workspace navigation polish may be refined during implementation.

PR #3 was merged to main with commit 07f9e0313bebb5ad604c9fd51f031a303edfc647.

## Phase handoff

Phase 2 is closed with the exception above. Phase 3 application-shell work is authorized. Do not claim that the deferred clickable prototype exists.
