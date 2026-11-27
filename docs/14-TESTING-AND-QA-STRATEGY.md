# SDC Studio — Testing and QA Strategy

**Status:** Draft  
**Purpose:** Define how SDC Studio will test its own platform and user projects, capture evidence, repair defects, and report verified status.

## 1. Testing principles

1. Testing is selected from project technology, task risk, requirements, and available tooling.
2. No single test category proves overall quality.
3. A test is reported as passed only when it actually ran and the runner recorded a successful outcome.
4. Skipped, unavailable, blocked, and not-run tests are distinct from passed.
5. Test evidence must be linked to a project, task, workspace revision, test run, and environment.
6. Repairs must be followed by relevant retesting and regression checks.
7. Browser screenshots show visual state but do not prove backend or business logic correctness.
8. The QA agent evaluates acceptance criteria independently from the developer agent's claims.
9. Test execution occurs in an isolated project workspace.
10. The owner can configure risk/coverage and avoid unnecessary checks unrelated to the task.

## 2. Test layers

### 2.1 Static analysis and code quality

Examples:
- TypeScript type checking.
- ESLint and formatting checks.
- Framework/build-time static checks.
- Code complexity and duplicate-code analysis where useful.
- Secret detection.
- Dependency and license analysis.
- Semgrep or equivalent static security analysis when configured.

Evidence:
- Exact command/tool/version.
- Workspace commit/revision.
- Exit code and duration.
- Findings count and severity.
- Redacted output/report artifact.

### 2.2 Unit tests

Purpose: Verify pure functions, validators, state transitions, permission policies, routing decisions, reducers, and isolated business logic.

Examples:
- Vitest/Jest for TypeScript/JavaScript.
- Framework-appropriate test runners for other languages.
- Property-based tests for complex validators/state machines when justified.

Evidence:
- Test suite and case counts.
- Passed/failed/skipped counts.
- Duration.
- Failure details and report artifact.

### 2.3 Integration tests

Purpose: Verify boundaries between modules and external adapters.

Examples:
- Database repositories and migrations.
- Provider adapter request/response normalization.
- GitHub adapter permission mapping using fixtures or a test repository.
- Tool gateway policy enforcement.
- Artifact metadata and storage access.
- Runner job submission and result handling.

Do not use live production credentials in tests. External adapters should support mocks, deterministic fixtures, and optional isolated test credentials.

### 2.4 End-to-end tests

Purpose: Verify complete operator workflows.

Initial journeys:
- Create blank project.
- Create a conversation and submit a planning request.
- Select a model and mode.
- Attach a supported file/image.
- View task plan and progress.
- Connect GitHub and select an installed repository.
- Run read-only repository discovery.
- Create task branch and display a file diff.
- Review an approval request.
- Execute a test in an isolated runner.
- View final report and evidence.

### 2.5 Browser and visual QA

Purpose: Verify rendered behavior in supported browser engines/viewports.

Checks:
- Page loads and navigation works.
- Primary interaction journeys complete.
- Responsive layout at configured desktop, tablet and mobile viewports.
- Keyboard navigation and focus visibility.
- Dialog, dropdown, composer, file attachment and preview states.
- Empty, loading, error, blocked and success states.
- Screenshot capture for approved checkpoint pages.
- Visual comparison against an explicitly identified baseline where available.

### 2.6 Accessibility testing

Checks may include:
- Semantic headings and landmarks.
- Accessible names/labels.
- Keyboard navigation and focus order.
- Focus trap/return for dialogs.
- Color contrast.
- Form error associations.
- Screen-reader-relevant status updates.
- Automated axe checks plus manual review for important workflows.

Automated accessibility findings are not equivalent to a complete accessibility audit.

### 2.7 Security testing

Checks:
- Authentication and authorization boundaries.
- Cross-project data isolation.
- Repository installation scope.
- Secret redaction and secret non-exposure.
- Prompt-injection tests using hostile repository/web content.
- Path traversal and symlink handling.
- Command and container isolation.
- Resource/time/network limits.
- Preview SSRF/internal-network protections.
- Approval enforcement for production/admin/destructive actions.
- GitHub/provider rate-limit behavior.
- Artifact access control and retention.

### 2.8 Performance testing

Applicable measurements:
- UI route and API latency.
- Task event streaming responsiveness.
- Model time-to-first-token and completion latency when provider reports it.
- Runner startup time.
- Project install/build/test duration.
- Preview readiness time.
- Browser page metrics and Lighthouse results where relevant.
- Memory/CPU/disk usage and process cleanup.

Do not define arbitrary universal thresholds before baseline measurement. Record the project/runtime context and use agreed thresholds for each target workflow.

## 3. Test discovery and profiles

The runner should inspect project manifests and existing documentation to identify:
- Language and runtime.
- Package manager.
- Install scripts.
- Build scripts.
- Lint/typecheck scripts.
- Unit/integration/E2E test commands.
- Development server/start commands.
- Existing browser configuration.
- Test and coverage output formats.

The agent proposes a test profile. The operator or approved project configuration determines permitted commands and resources. Unknown lifecycle scripts are treated as executable untrusted code and run only in isolation.

Profiles:
- **Quick:** Static checks and focused unit tests for iteration.
- **Standard:** Build, lint/typecheck, relevant unit and integration tests.
- **Full QA:** Standard plus E2E, browser, accessibility, regression and selected security/performance checks.
- **Custom:** Explicit project/owner-defined tests.

The profile name does not guarantee that all test categories exist. The report lists what ran and what was not available.

## 4. Bug reproduction and repair loop

1. Capture the initial failure and exact environment/revision.
2. Reproduce the issue using a deterministic test or browser steps where possible.
3. Gather relevant logs, screenshots, traces, network events, and source context.
4. Form a root-cause hypothesis with confidence and evidence.
5. Identify the smallest task-scoped correction.
6. Check file/resource permission and apply the change in the isolated workspace.
7. Run the failing test first.
8. Run affected integration/regression checks.
9. Run broader verification according to the selected profile.
10. Compare screenshots where the issue is visual.
11. Report fixed/unfixed/partially fixed state with evidence.

The loop is bounded by configurable attempt, time, token, and resource budgets. On repeated failure, stop and report the remaining blocker rather than endlessly changing code.

## 5. Browser evidence specification

Each browser test record should include:
- Project ID and task ID.
- Workspace revision or commit SHA.
- Preview origin/URL.
- Browser engine and version.
- Viewport dimensions/device profile.
- Journey/test identifier.
- Start/end timestamp and duration.
- Navigation result.
- Console errors and warnings where captured.
- Uncaught page errors.
- Failed requests and response status information.
- Screenshot and trace artifact references.
- Result state and failure summary.

Sensitive headers, cookies, access tokens, and private URL parameters must be redacted.

## 6. QA report structure

A task QA report includes:

### Summary
- Overall status.
- Tested revision.
- Selected profile.
- Start/end time.

### Test matrix
- Test category.
- Command/profile.
- Status: passed, failed, skipped, blocked, not run.
- Passed/failed/skipped counts where available.
- Duration.
- Evidence reference.

### Browser evidence
- Preview URL (scoped and access-controlled).
- Browser and viewport.
- Screenshot links.
- Console/page errors.
- Failed requests and relevant statuses.

### Defect and repair history
- Reproduction steps.
- Severity and affected requirement.
- Root-cause hypothesis and evidence.
- Files changed.
- Retest outcome.

### Remaining limitations
- Untested features.
- Environment constraints.
- Disabled tests.
- Known warnings.
- Residual risks.

### Recommendation
- Whether acceptance criteria are satisfied.
- Whether owner review is needed.
- Next action directly related to the task.

## 7. Evidence integrity

- Link every result to actual runner output.
- Store a hash for retained artifacts where practical.
- Record the tested source revision.
- Do not silently replace screenshots/traces.
- Keep failed runs; a later passed run supplements rather than erases history.
- Distinguish a model's interpretation from the raw test result.
- Do not report a browser console as error-free unless the configured capture actually ran.

## 8. Phase-level validation policy

Every implementation phase defines its own test plan and exit evidence. During Phase 1, no application tests are applicable because no application code exists. Documentation consistency checks include link validation, ID/reference validation, terminology checks, scope checks, and owner review.

## 9. Definition of QA completion

QA is complete for a task only when:
- Its acceptance criteria are mapped to executed checks or explicitly marked not testable.
- Applicable tests have run and statuses are recorded.
- Failures have been fixed/retested or explicitly disclosed.
- Browser/screenshot evidence is linked where applicable.
- The tested source revision is known.
- Residual risks and untested areas are clear.
- The final report does not overstate coverage.
