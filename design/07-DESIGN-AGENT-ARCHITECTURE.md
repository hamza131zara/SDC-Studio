# SDC Studio — AI Design Workflow and Integration Architecture

**Status:** Phase 2 design proposal  
**Work item:** P2-11  
**Purpose:** Define how SDC Studio itself will design websites, applications, dashboards, DApps, APIs, LLM tools, and other software products. This document defines the intended design subsystem, not its implementation.

## 1. Problem statement

A general-purpose language model can suggest layouts and generate code, but a polished product design requires more than a prompt and a screenshot. A reliable design workflow needs:
- Requirements and user context before visual styling.
- Information architecture and task flows.
- Wireframes before high-fidelity screen design.
- A reusable design system and component rules.
- Responsive and accessible states.
- Editable design artifacts rather than one flattened image.
- Interactive prototypes for reviewing behavior.
- Visual critique against requirements and screenshots.
- A controlled handoff from design to implementation.

SDC Studio must therefore treat product design as a structured SDLC discipline with dedicated artifacts, specialist roles, tool adapters, review gates, and traceability.

## 2. Design subsystem goals

The Design subsystem should let an operator:
1. Describe a product, feature, or existing interface in natural language.
2. Attach reference images, sketches, brand assets, documents, screenshots, or an existing repository.
3. Generate and revise product requirements, personas/roles, information architecture, and user journeys.
4. Create low-fidelity wireframes and high-fidelity screens.
5. Establish reusable components, variants, typography, colors, spacing and design tokens.
6. Build an interactive prototype that demonstrates primary journeys.
7. Review desktop, tablet, and mobile states.
8. Run design/accessibility/consistency critiques and request revisions.
9. Export or synchronize editable artifacts with a supported design platform.
10. Hand approved design tokens, screen specs, assets and interaction contracts to the Development phase.

The output must be useful even when no external design account is configured.

## 3. Core design principles

### 3.1 Design is a staged process

Do not jump directly from a product prompt to final high-fidelity screens. The normal sequence is:

1. Brief and constraints.
2. Requirements and target users.
3. Information architecture.
4. User journeys and task flows.
5. Low-fidelity wireframes.
6. Visual direction and design tokens.
7. High-fidelity screens and reusable components.
8. Responsive and state variants.
9. Interactive prototype.
10. Design critique and accessibility review.
11. Owner approval.
12. Handoff package for implementation.

The operator may request a shorter path for a small task, but the system must disclose which design stages were skipped and what risks that creates.

### 3.2 Editable design artifacts

A screenshot is evidence, not the source design. The design representation must preserve editable screen/frame structure, component identity, text, layout, variants, tokens, and prototype links. Full-screen raster images must not be used as a substitute for editable UI design.

Images may be used for photographs, illustrations, textures, icons, logos or other genuine visual assets. They must be separate assets with provenance and appropriate usage rights.

### 3.3 Human direction with AI execution

The model proposes designs, explains decisions and performs authorized revisions. The owner can approve, reject, annotate, or request alternatives. The agent must not interpret an unreviewed design draft as a production-approved specification.

### 3.4 Evidence-based quality

Design critique must reference concrete evidence: an affected screen/frame, component, token, viewport, requirement, or accessibility criterion. Avoid claiming that a UI is "professional" merely because a model says so.

## 4. Logical architecture

    Operator brief + files + references + optional repository
                         |
                         v
                 Design Workspace
                         |
                         v
                Design Orchestrator
                  /      |       \
                 /       |        \
       UX Architect  Visual Designer  Design Systems Specialist
                 \       |        /
                  \      |       /
                   Design Artifact Model
                         |
              +----------+-----------+
              |          |           |
              v          v           v
        Native Canvas  Figma Adapter  Penpot Adapter
        / Prototype    (when enabled) (when enabled)
              |
              v
       Screenshot / Prototype Capture
              |
              v
       Design Critic + Accessibility Reviewer
              |
              v
       Owner Review / Approval Gate
              |
              v
       Versioned Design Handoff Package
              |
              v
       Phase 3+ Implementation Workflow

The design orchestrator coordinates specialist work. External design tools are adapters and destinations, not mandatory sources of truth.

## 5. Specialist roles

### DS-01 — UX Architect

Owns:
- Product brief clarification.
- User roles and needs.
- Information architecture.
- User journeys and task flows.
- Navigation and content hierarchy.
- Interaction requirements and edge cases.

Outputs: approved brief draft, IA, journey map, and interaction requirements.

### DS-02 — Visual/UI Designer

Owns:
- Visual exploration and screen composition.
- Typography, color, hierarchy, spacing, density and visual consistency.
- Responsive layout direction.
- Empty, loading, error, blocked, success and disabled states.
- Visual asset direction.

Outputs: visual direction, screen designs, responsive variants and design annotations.

### DS-03 — Design Systems Specialist

Owns:
- Design tokens and semantic variables.
- Component inventory and hierarchy.
- Variants, reusable component states and usage rules.
- Design-to-code naming and handoff consistency.

Outputs: token catalog, component catalog, variant rules and implementation mapping.

### DS-04 — Accessibility and Design Critic

Owns:
- Contrast and visual hierarchy review.
- Keyboard/focus and semantic interaction review.
- Responsive and content-overflow critique.
- Consistency checks against approved design tokens and requirements.
- Evidence-backed improvement suggestions.

Outputs: prioritized design findings, affected nodes/screens, severity, and suggested corrections.

### DS-05 — Prototype and Handoff Specialist

Owns:
- Connecting screens and interactions into a prototype.
- Defining navigation, transitions and interaction outcomes.
- Preparing screen specs, component metadata, assets and tokens for implementation.
- Recording incomplete or simulated behavior.

Outputs: interactive prototype, screen inventory, handoff package and known limitations.

These are logical roles. The orchestrator may use one capable model for several roles or route roles to different configured models. Do not require separate paid agents.

## 6. Design artifact model

The design subsystem should maintain a normalized internal representation independent of a specific canvas vendor.

Core logical records:

- **DesignProject:** project ID, name, description, source project/repository, owner, status, active version.
- **DesignBrief:** product goal, target users, platform, constraints, brand input, technical assumptions, acceptance criteria.
- **DesignRequirement:** stable ID, expected user outcome, priority, source requirement ID.
- **DesignFlow:** journey/flow ID, ordered steps, screens, decisions, transitions and error paths.
- **DesignScreen:** screen ID, name, route/context, target platform, viewport class, state, linked requirements.
- **DesignNode:** semantic element type, parent, order, text, asset reference, geometry/layout, token references, component reference, accessibility metadata.
- **DesignComponent:** component ID, purpose, properties, variants, states, usage rules.
- **DesignToken:** token name, semantic category, value by theme, optional alias and usage notes.
- **PrototypeLink:** source screen/node, trigger, destination screen/state, transition and interaction outcome.
- **DesignAsset:** asset ID, media type, source/provenance, usage rights note, dimensions and file reference.
- **DesignReview:** reviewer role, target screen/node, requirement, finding, severity, evidence and disposition.
- **DesignVersion:** version number, author/agent, changes, parent version, created time and approval state.
- **DesignHandoff:** approved version, screen inventory, tokens, components, assets, interaction notes and unresolved items.

The concrete database schema belongs to the later persistence implementation. Phase 2 defines semantics and relationships only.

## 7. Design tool adapter strategy

### 7.1 Native SDC Studio Design Workspace

This is the mandatory baseline. SDC Studio must have an internal design artifact model and a way to preview designs, so core design work does not depend on a paid third-party account.

The native workspace should eventually support:
- Structured screen/frame editing.
- Components and variants.
- Theme tokens.
- Screen linking and prototype flow.
- Preview at selected viewport sizes.
- Import/export of standard assets and design metadata.
- Version history and review comments.

Implementation of the native editor itself is a later development task and must be scoped carefully. Do not attempt to build a full Figma clone before validating the minimum authoring and preview needs.

### 7.2 Figma adapter

Figma is the preferred professional external design destination when the owner has sufficient edit access.

Desired capabilities when available:
- Create or select a design file.
- Write editable frames, components, variables and styles.
- Read existing file structure, tokens and components.
- Synchronize approved design versions.
- Export screenshots/assets and retrieve handoff context.
- Link the Figma file and relevant frames to a project/task.

The Figma MCP connection provides structured design context and can support writing to editable canvas on supported plans/seats. It is not a guarantee that a model will create a good design; model output still requires design-system context, visual review and owner approval.

**Current integration assessment (2026-11-27):** The ChatGPT environment exposes Figma and Canva connectors. The connected Figma identity currently reports View seats on Starter plans. Figma's published guidance says canvas-writing through its MCP workflow is available on supported Professional, Education, Organization and Enterprise plans; basic inspection has different access/limits. Therefore SDC Studio must not assume this current connection can author Figma canvas content. Recheck the plan/seat and actual permissions before enabling write operations.

References:
- Figma MCP introduction: https://developers.figma.com/docs/figma-mcp-server/
- Figma MCP access and limits: https://developers.figma.com/docs/figma-mcp-server/rate-limits-access/
- Figma Starter plan: https://help.figma.com/hc/en-us/articles/13838684089751-Starter-plan-overview

### 7.3 Penpot adapter

Penpot is the recommended open-source/free-first external design alternative to evaluate when Figma canvas authoring is unavailable or unaffordable.

Desired adapter capabilities:
- Export/import supported SVG and design assets.
- Link a Penpot design/prototype to a project.
- Read design metadata through available supported APIs.
- Synchronize tokens/components where supported by actual API capability.
- Preserve a platform-neutral copy of the design artifact model.

Penpot's public platform describes UI design, interactive prototypes, design systems, tokens, components, variants, open formats and self-hosting. Exact integration capabilities must be verified before implementation; do not assume feature parity with Figma.

Reference: https://penpot.app/

### 7.4 Canva and other visual tools

Canva is useful for brand collateral, illustrations, marketing material, presentations and visual assets. It is not the default source of truth for complex application screen structure, component variants or engineering handoff.

Optional AI design/prototyping products such as Product Design and MagicPath may help the owner explore ideas or create review prototypes in ChatGPT. They are external assistants, not core SDC Studio dependencies unless a supported API/connector contract is later verified.

### 7.5 Adapter policy

- Keep Figma/Penpot/other integrations behind a common adapter contract.
- Do not send private source code or secrets to design providers unless the owner explicitly authorizes that data flow.
- Distinguish read, create, edit, export and comment capabilities.
- Record integration availability, permission scope, rate limits and errors.
- A missing external tool must not prevent design work in the native artifact model.
- Do not claim a synchronization completed until the external tool confirms it.

## 8. Model capability requirements

Design tasks benefit from models with:
- Strong instruction following and long-context reasoning.
- Image/screenshot understanding.
- Structured output/JSON schema support.
- Reliable tool calling.
- Ability to critique layout and content hierarchy.
- Ability to compare design references with implementation screenshots.

The AI gateway must expose actual capabilities rather than label every model as a design expert. For a text-only model, use a vision-capable configured model for screenshot critique when available. If no suitable vision model is configured, report that visual critique is unavailable and continue with structural checks.

## 9. End-to-end design workflow

### Step 1 — Design brief
Collect intended product, target users, platform(s), primary tasks, constraints, required screens, references, brand assets and acceptance criteria.

### Step 2 — Analyze and research
Inspect supplied materials and, when authorized, current reference products/patterns. Cite research. Do not copy proprietary designs wholesale.

### Step 3 — Requirements and UX structure
Generate requirements, information architecture, user journeys, screen inventory and edge cases. Link each screen to a requirement or explain its role.

### Step 4 — Wireframe
Create low-fidelity screen layouts. Validate that information hierarchy and task flows are complete before visual polish.

### Step 5 — Visual system
Establish color and theme tokens, typography, spacing, grid, surfaces, iconography, breakpoints, component states and accessibility rules.

### Step 6 — High-fidelity screens
Generate editable semantic UI elements with reusable components and realistic content. Avoid complete-screen raster output.

### Step 7 — Prototype
Link screens to represent primary user journeys. Use clearly labeled simulated data when no backend exists.

### Step 8 — Critique
Run structural, consistency, responsive and accessibility reviews. Use a vision-capable model to inspect rendered screens when available.

### Step 9 — Revise
Prioritize high-impact issues, revise only affected elements, preserve accepted design decisions and record version changes.

### Step 10 — Owner approval
Present the prototype, screen index, design decisions, known limitations and outstanding questions. Do not mark design approved until the owner accepts it.

### Step 11 — Handoff
Export approved tokens, components, screen specs, assets, responsive rules, interactions, accessibility annotations and requirement links.

## 10. Design quality review framework

The review should score each relevant dimension from 1–5 and attach evidence:
- Requirement coverage.
- Information hierarchy.
- Navigation and task clarity.
- Visual consistency.
- Component reuse.
- Typography and readability.
- Responsive behavior.
- Accessibility.
- Interaction completeness.
- Implementation handoff quality.

A numeric score is a review aid, not proof of quality. The report must include findings and examples, and the owner remains the final approver.

Critical findings should include:
- Missing primary user task.
- Inconsistent navigation.
- Unreadable/overlapping content.
- Broken prototype links.
- Missing critical states.
- Poor text/control contrast.
- Mobile overflow or unusable interactions.
- Inconsistent component/token usage.
- Handoff gaps that would force developers to guess.

## 11. Design-to-development handoff contract

Each approved handoff includes:
1. Design version and approval record.
2. Information architecture and user flows.
3. Screen inventory with stable screen IDs.
4. Desktop/tablet/mobile layout rules.
5. Theme and design tokens.
6. Component catalog and variants.
7. Interaction and state specifications.
8. Asset list with source and usage notes.
9. Accessibility criteria.
10. Prototype link and representative screenshots.
11. Requirement traceability.
12. Known limitations and deferred design decisions.

Development agents must follow the approved tokens/components and may propose a design deviation only with an explicit reason and owner review.

## 12. Security and intellectual property

- Treat user-uploaded references and imported design files as untrusted data.
- Do not execute code embedded in an asset or imported design.
- Do not use confidential assets in an external provider unless authorized.
- Record asset source and licensing/usage notes where known.
- Do not copy a third-party product's proprietary brand or complete visual identity.
- Never put provider keys or design-provider access tokens in model prompts or generated frontend code.

## 13. Phase 2 versus implementation boundary

Phase 2 must define this design architecture, experience, data concepts, interaction model and handoff process.

Later implementation phases may build the native design workspace, design artifact persistence, external adapters and visual preview tooling. This document does not authorize building those features during Phase 2.

## 14. Acceptance conditions

- The design process is staged and traceable.
- Design outputs are editable artifacts, not only screenshots.
- Figma and Penpot are treated as adapters, not hard dependencies.
- The native design artifact model allows work to continue without external authoring access.
- Model capability and vision availability are represented accurately.
- Screens, tokens, components and prototype flows link back to requirements.
- Visual and accessibility review produces evidence-backed findings.
- Owner approval is required before handoff is marked accepted.
- The architecture has a clear boundary between Phase 2 design specification and later implementation.


## 15. Product Design capability — assisted design workflow

**Availability:** Product Design is installed in ChatGPT. Its full workflow is designed for ChatGPT Work mode; the installed skill instructions say standard Chat mode is not supported for the full Product Design workflow.

Use Product Design in Work mode to assist with:
- Confirming and refining the product design brief.
- Exploring alternative product/UI directions.
- Auditing user flows and screen structure.
- Prototyping from a live URL when one exists.
- Turning static screenshots into interactive prototype experiences.

For the current Phase 2:
1. Use the accepted Phase 0/1 documents and [Prototype Design Brief](./08-PROTOTYPE-DESIGN-BRIEF.md) as canonical input.
2. Use Product Design in Work mode to challenge unclear requirements and explore visual directions.
3. Review any flow audit against the approved information architecture and acceptance criteria.
4. Use prototype capabilities where available. There is no live SDC Studio URL yet, so begin from the brief and editable design references.
5. Preserve approved decisions and record changes in the design version/decision record.
6. Bring reviewed output back into the Phase 2 package and owner approval process.

Boundaries:
- Product Design is a ChatGPT design aid; it is not an SDC Studio runtime dependency.
- Do not assume it exposes a callable API for SDC Studio.
- Generated screens/prototypes are proposals until the owner accepts them.
- Preserve editable structure/components/tokens where supported; static screenshots are references, not the canonical design.
- Prototype data must be clearly labelled as sample data.

## 16. Current design-tool access and recommended workflow

**Snapshot date:** 2026-11-27. Recheck account permissions, free plans, and quotas before relying on any external service.

### 16.1 Figma — installed, but current seat is view-only

The ChatGPT Figma app is installed and connected. The authenticated Figma account lookup returned two Starter team memberships, both with a **View** seat.

Figma's published seat matrix states that View seats can view/comment but cannot edit Figma Design files. Figma's remote MCP guide states that writing to the canvas outside Drafts requires a Full seat. Figma Make can be tried in Drafts by View-seat users, but Figma says Make files created by View seats cannot be shared without upgrading to a Full seat.

Therefore:
- Do not claim that SDC Studio's Figma Design screens have been authored or changed through the current View-seat connection.
- Use Figma for reading/inspection where permitted.
- Treat Figma Make-in-Drafts as a possible private exploration path only; verify current feature access and AI credit balance first.
- Re-evaluate Figma canvas-writing if the owner gets Full-seat/edit permissions or a new eligible workspace.

References:
- [Figma seat comparison](https://help.figma.com/hc/en-us/articles/360039960434-Compare-and-manage-seats-in-Figma)
- [Figma MCP server: write to canvas and access](https://developers.figma.com/docs/figma-mcp-server/)
- [Figma Make access](https://help.figma.com/hc/en-us/articles/31722591905559-Figma-Make-FAQs)

### 16.2 Penpot — preferred free-first editable UI canvas to evaluate

Penpot is the preferred first option to evaluate for editable UI design without purchasing a Figma Full seat.

Reasons:
- Penpot describes its Professional cloud plan as free and fully featured for core design work.
- Penpot is open source and supports UI design, interactive prototypes, design systems, tokens, components and variants.
- Penpot publishes an official MCP server that can read and modify Penpot file structure, including pages, layers, components, styles and tokens.

Important implementation boundary:
- The Penpot MCP server is not currently connected as a tool in this ChatGPT session.
- Do not claim it is connected or create/modify Penpot files until the owner enables the Penpot MCP server and connects an MCP-compatible client.
- Verify current cloud storage, team limits, and server capabilities during setup; don't promise feature parity with Figma.

Suggested setup:
1. Create/sign in to a free Penpot account at [design.penpot.app](https://design.penpot.app).
2. Enable the MCP server under the Penpot account integrations.
3. Open a Penpot design file and connect the Penpot MCP plugin to that file.
4. Connect the returned remote MCP endpoint/session to a supported MCP client, or use Penpot's documented local setup.
5. Begin read-only (list pages, inspect components/tokens) before allowing edits.
6. Build the SDC Studio design system and key screens as native editable objects.

Official references:
- [Penpot pricing](https://penpot.app/pricing)
- [Penpot MCP documentation](https://help.penpot.dev/mcp/)
- [Penpot open-source platform](https://penpot.app/)

### 16.3 ChatGPT Product Design — installed; full workflow requires Work mode

The Product Design capability is available in the ChatGPT plugin environment, but its installed workflow instructions state that full ideation/prototyping workflows are not run in standard Chat mode. Use ChatGPT **Work mode** for the full Product Design experience.

Use it for:
- Visual direction exploration.
- Product brief refinement.
- User-flow audits and UX critique.
- Image/screenshot-based ideation.
- Interactive prototype workflows where supported.

Do not treat it as a guaranteed Figma editor or a permanent SDC Studio runtime dependency. A generated prototype remains a proposal until the owner reviews it.

### 16.4 MagicPath — optional, not the current free-first dependency

MagicPath is installed and its tool connection is available. The latest account context returned zero available credits and no free plan available for this account. Therefore, do not plan the current Phase 2 deliverable around MagicPath AI generation unless the owner later confirms available credits or a plan.

MagicPath remains an optional alternative for multi-screen design exploration, collaborative canvas work and code export if access/credits become available.

Reference: [MagicPath documentation](https://www.magicpath.ai/documentation).

### 16.5 Canva — supporting visual assets, not the primary UI design source

Canva is installed and useful for brand collateral, social graphics, presentations, diagrams and marketing assets. It is not the default source of truth for complex application screen hierarchies, component variants, design tokens, or code handoff.

### 16.6 tldraw — architecture and flow diagrams

The tldraw ChatGPT app has been suggested for connection. It is intended for drawing and diagramming with an AI agent on a canvas.

Use it for:
- System/context diagrams.
- Agent orchestration and tool-flow diagrams.
- User journey and decision-flow sketches.
- Architecture review workshops.

It is optional for Phase 2 and is not a replacement for a UI design system or clickable product prototype. Do not assume it is connected until the owner completes the app connection.

### 16.7 Recommended working method for SDC Studio

1. **GPT/architecture work:** Translate requirements into user journeys, IA, screen inventory, design constraints, design tokens, and acceptance criteria.
2. **Product Design in Work mode:** Explore directions, challenge flows, and critique visual hierarchy/accessibility.
3. **Penpot as the free-first editable canvas:** Build the approved component system, screens and prototype when its MCP connection is ready.
4. **Figma as an optional professional destination:** Use it when Full-seat/edit access becomes available or when the owner selects a suitable editable draft workflow.
5. **tldraw for architecture diagrams:** Keep system and agent diagrams distinct from final UI screen designs.
6. **Owner review:** Select the final direction and approve the prototype.
7. **Design handoff:** Store the approved design URL, frame IDs, tokens, interaction notes and acceptance mapping in the repository before Phase 3.

No single plugin makes GPT a professional designer by itself. Quality depends on strong requirements, reference research, visual alternatives, a real editable design canvas, repeated critique, usability/accessibility checks, and owner approval.
