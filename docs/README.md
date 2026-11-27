# SDC Studio — Documentation Index

**Status:** Phase 1 documentation draft for owner review  
**Approved foundation:** Phase 0 is merged into main  
**Active branch:** phase-1/documentation  
**Active PR:** #2 — Phase 1: SDC Studio documentation

This folder is the detailed reference set for requirements, workflows, architecture, implementation planning, and ongoing project continuity.

## Product and engineering specifications

1. [Product specification](./01-PRODUCT-SPECIFICATION.md)
2. [User workflows](./02-USER-WORKFLOWS.md)
3. [System architecture](./03-SYSTEM-ARCHITECTURE.md)
4. [Module contracts](./04-MODULE-CONTRACTS.md)
5. [Conceptual data model](./05-DATA-MODEL.md)
6. [Security specification](./06-SECURITY-SPECIFICATION.md)
7. [Engineering standards](./07-ENGINEERING-STANDARDS.md)
8. [Acceptance criteria](./08-ACCEPTANCE-CRITERIA.md)

## Project control and implementation planning

9. [Master implementation roadmap and backlog](./09-IMPLEMENTATION-ROADMAP.md)
10. [Project status register](./10-PROJECT-STATUS.md)
11. [Requirements traceability matrix](./11-REQUIREMENTS-TRACEABILITY.md)
12. [Architecture and product decision log](./12-DECISION-LOG.md)
13. [Agent roles and tool catalog](./13-AGENT-AND-TOOL-CATALOG.md)

## Quality and integrations

14. [Testing and QA strategy](./14-TESTING-AND-QA-STRATEGY.md)
15. [External integration strategy](./15-INTEGRATION-STRATEGY.md)
16. [Project memory and change-control guide](./16-PROJECT-MEMORY-AND-CHANGE-CONTROL.md)

## Foundation document

The approved Phase 0 baseline remains at the repository root: [PHASE-0-FOUNDATION.md](../PHASE-0-FOUNDATION.md).

## How to use these documents

- For current progress and the immediate next action, read 10-PROJECT-STATUS.md first.
- For what to implement and in what order, use 09-IMPLEMENTATION-ROADMAP.md.
- For a specific requirement, use 11-REQUIREMENTS-TRACEABILITY.md and then open its authoritative specification.
- For why a technology or policy was selected, use 12-DECISION-LOG.md.
- For agent/tool behavior, use 13-AGENT-AND-TOOL-CATALOG.md.
- For resuming after a chat/session change, follow 16-PROJECT-MEMORY-AND-CHANGE-CONTROL.md.

## Phase boundary

Phase 1 formalizes the approved Phase 0 foundation. It does not authorize UI design, application implementation, provider integration, sandbox implementation, or deployment implementation. Those belong to later approved phases.

Phase 2 — UI/UX Design begins only after Phase 1 documentation is reviewed, accepted, and merged.

## Change control

Keep these documents consistent. Record material corrections in the Phase 1 branch and update affected cross-references. Do not expand the product scope with unrelated features. Owner approval is required before merging the phase documentation and proceeding to the next phase.
