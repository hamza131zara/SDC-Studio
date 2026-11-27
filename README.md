# SDC-Studio

Private AI-powered software engineering platform for agency workflows. Multi-agent AI, GitHub integration, SDLC planning, documentation, UI/UX design, development, automated testing, live previews, debugging, QA, deployment and maintenance.

## Project status

- Phase 0 — Foundation: **accepted and merged**
- Phase 1 — Documentation: **approved, merged and frozen** (PR #2)
- Current phase: **Phase 3 — Application Shell**
- Phase 2 status: **written design package accepted and merged; clickable prototype explicitly waived and deferred by owner**
- Working branch: phase-3/application-shell
- Default branch: main
- Application implementation: P3-01 repository/app bootstrap in progress
- UI/UX design: written specifications accepted; P2-09 interactive prototype not created and remains deferred

## Approved foundation

See [Phase 0 Foundation](./PHASE-0-FOUNDATION.md) for product scope, the 10/10 legitimate engineering user-freedom target, architecture baseline, security model, repository policy, and phase sequence.

## Current design work

Phase 2 written design package: [PR #3](https://github.com/hamza131zara/SDC-Studio/pull/3) — merged on 2026-11-27 (merge commit 07f9e0313bebb5ad604c9fd51f031a303edfc647). The owner accepted the written information architecture, journeys, wireframes, design system, interaction/responsive specifications, and design-agent architecture. The P2-09 clickable prototype was explicitly waived for this milestone and remains deferred; it was not created.

## Project documentation

Start at the [Documentation Index](./docs/README.md). It links the complete product specification, user workflows, system architecture, module contracts, data model, security requirements, engineering standards, acceptance criteria, detailed roadmap, status register, traceability matrix, decision log, agent/tool catalog, QA strategy, integration strategy, and project continuity guide.

## Where to resume

1. Read [Project Status](./docs/10-PROJECT-STATUS.md) to see what is complete, pending, blocked, and next.
2. Read [Implementation Roadmap](./docs/09-IMPLEMENTATION-ROADMAP.md) for phase order, tasks, dependencies, and exit criteria.
3. Read [Project Memory Guide](./docs/16-PROJECT-MEMORY-AND-CHANGE-CONTROL.md) before resuming after a break or assistant/session change.

## Phase policy

Work is phase-gated. Do not start the next phase until the current phase has been reviewed and accepted by the project owner. Phase 1 documentation is approved and frozen. Phase 2 written design specifications were accepted by the owner on 2026-11-27 with an explicit waiver of the clickable prototype; Phase 3 application-shell work is authorized. The prototype remains deferred and must not be described as completed.

## Repository safety

- Keep main stable.
- Work on task-specific branches and review diffs.
- Do not force-push or rewrite shared history.
- Do not commit credentials, API keys, tokens, or secret-bearing environment files.
