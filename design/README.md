# SDC Studio — Phase 2 UI/UX Design Package

**Phase:** 2 — UI/UX Design  
**Status:** Design draft for owner review  
**Branch:** phase-2/ui-ux-design  
**Foundation:** Phase 0 and Phase 1 accepted and merged into main  
**Boundary:** Design artifacts only. No application source code, backend implementation, AI provider integration, GitHub App implementation, runner, preview engine, or deployment code is part of this phase.

## Design package

1. [Information Architecture](./01-INFORMATION-ARCHITECTURE.md)
2. [User Journeys](./02-USER-JOURNEYS.md)
3. [Screen Wireframes](./03-SCREEN-WIREFRAMES.md)
4. [Visual Design System](./04-DESIGN-SYSTEM.md)
5. [Interaction and Responsive Specification](./05-INTERACTION-RESPONSIVE-SPEC.md)
6. [Design Acceptance Criteria](./06-DESIGN-ACCEPTANCE.md)

## Core experience

SDC Studio combines:
- A ChatGPT/Claude-style conversation experience.
- A project-centric workspace connected to blank/local/GitHub repositories.
- An IDE-style surface for files, diffs, terminal, preview, QA, evidence, and reports.
- A visible agent activity timeline and owner-controlled approval flow.

The interface must keep the conversation as the primary way to express intent while giving the operator clear visibility into project state and agent operations.

## Design status

| Work item | Status |
|---|---|
| P2-01 Information architecture | Draft prepared |
| P2-02 Core user journeys | Draft prepared |
| P2-03 Main application shell | Draft prepared |
| P2-04 Chat experience | Draft prepared |
| P2-05 Project workspace | Draft prepared |
| P2-06 Execution transparency | Draft prepared |
| P2-07 Settings and integrations | Draft prepared |
| P2-08 Design system | Draft prepared |
| P2-09 Interaction prototype specification | Draft prepared |
| P2-10 Design review and acceptance | Pending owner review |

## Design assumptions requiring owner review

- Desktop-first composition because the primary workflow involves code, diffs, terminal, and preview side-by-side.
- Responsive tablet/mobile support for conversation, task status, reports, and approvals; full multi-pane development is optimized for desktop.
- Dark developer-focused default theme with a light theme as a design-system variant.
- Neutral visual foundation; final brand accent and logo can be aligned with SDC branding during owner review.
- One operator-focused private product; no public signup, billing, team administration, or customer tenancy screens.

These assumptions do not change the approved product scope. Owner feedback may adjust visual styling and layout within the existing product requirements.

## Phase gate

Phase 2 is complete only after the owner reviews and accepts the design package. Phase 3 application shell implementation must not begin before this approval.
