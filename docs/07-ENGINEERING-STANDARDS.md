# SDC Studio — Engineering Standards

**Status:** Draft  
**Applies to:** Future implementation and maintenance phases  
**Purpose:** Keep the codebase understandable, testable, secure, and maintainable without unnecessary process overhead.

## 1. Technology and repository standards

- Use TypeScript for the initial application and shared packages.
- Use a pnpm workspace monorepo following the approved Phase 0 folder ownership.
- Use Next.js for the web application, Node.js for backend/runtime services, PostgreSQL for persistent relational data, Playwright for browser automation, and Docker-based isolation for project execution as specified in the architecture baseline.
- Keep dependencies current within compatibility constraints.
- Prefer established, maintained libraries over unnecessary custom implementations.
- Do not add a dependency or external service without a concrete requirement.
- Keep platform code separate from untrusted project code.

## 2. Module boundaries

- Web UI calls authenticated backend contracts; it does not execute project commands.
- Agent core coordinates plans and tools but does not directly call raw provider SDKs or filesystem/Git/terminal/browser implementations.
- AI gateway owns provider adapters and provider credentials.
- GitHub integration owns GitHub API access and installation-token handling.
- Execution package owns workspace and command abstractions.
- Browser QA owns Playwright lifecycle and browser evidence.
- Shared package contains reusable types and validation only; avoid business-logic dumping grounds.
- Cross-module calls use typed interfaces and validated data.

## 3. TypeScript and code quality

- Enable strict TypeScript settings.
- Avoid untyped external data; use explicit types, runtime validation, and safe narrowing at boundaries.
- Prefer small modules and functions with clear responsibility.
- Handle errors explicitly; do not silently swallow failures.
- Use structured error categories and safe user-facing messages.
- Avoid hidden side effects during module import.
- Use meaningful names and comments for non-obvious decisions.
- Keep formatting and linting automated and consistent.

## 4. API and event standards

- Version externally persisted and cross-module contracts.
- Validate request payloads server-side.
- Use stable error codes and redacted details.
- Scope every project/task lookup by authenticated ownership and authorization.
- Use idempotency identifiers for operations that may be retried.
- Record task-state transitions and tool outcomes.
- Use Server-Sent Events for initial real-time task updates unless implementation evidence requires a different approach.
- Do not expose secrets or internal stack traces to clients.

## 5. Git and pull request standards

- Keep main stable.
- Use phase/task branches, such as phase-specific design or implementation branches.
- Do not force-push or rewrite shared history.
- Do not commit directly to main through agent workflows.
- Keep pull requests focused and explain scope, behavior, tests, and risks.
- Review actual diffs for every repository mutation.
- Do not combine unrelated refactors with a requested feature.
- Preserve unrelated user changes.
- Use clear commit messages, preferably conventional prefixes: feat, fix, docs, test, refactor, chore, and security.

## 6. Testing standards

Applicable layers:

1. **Unit:** Pure logic, validators, state transitions, model-routing decisions, and permission rules.
2. **Integration:** Database repositories, provider adapters with fixtures, GitHub adapter boundaries, and tool contracts.
3. **Execution isolation:** Filesystem boundaries, container configuration, command/resource limits, and secret non-exposure.
4. **End-to-end:** Operator workflows across chat, projects, tasks, approvals, and reports.
5. **Browser QA:** Playwright journeys, screenshots, console/page errors, failed requests, and responsive viewports.
6. **Regression:** Verify existing requirements after changes.
7. **Security:** Authorization, secret redaction, prompt-injection boundaries, unsafe path/command attempts, and rate-limit behavior.
8. **Performance:** Measure important paths and resource use when performance requirements or regressions warrant it.

Rules:
- Tests must be repeatable and identify their environment.
- Never fabricate test outcomes.
- Skipped or unconfigured tests are not passes.
- Test failures include useful summaries and available evidence.
- Run the smallest relevant test set during iteration, then broader regression checks before completion.
- Store test output and screenshot/trace references against the task.
- Do not use live production credentials in automated tests.

## 7. Browser and visual evidence

- Capture screenshots at explicit checkpoints and viewports.
- Name evidence using project/task/run/step context.
- Record tested URL and application revision.
- Capture console and page errors, failed network requests, and navigation errors.
- Redact secrets and personal data from screenshots/logs where possible.
- Compare visual changes only against an identified baseline.
- Report browser coverage accurately.

## 8. Dependency and supply-chain standards

- Prefer maintained packages with suitable licensing.
- Review dependency additions for necessity, license, maintenance, and known vulnerabilities.
- Use lockfiles and reproducible installs.
- Run relevant dependency/security checks.
- Do not blindly execute unknown install/build scripts outside the isolated runner.
- Pin actions/dependencies where practical and review updates.
- Treat generated or downloaded code as untrusted until inspected.

## 9. Secrets and configuration

- Commit example variable names only, never secret values.
- Keep environment files, private keys, tokens, credentials, and signing material out of Git.
- Validate required configuration at startup with safe error messages.
- Centralize secret retrieval and redaction.
- Do not send secret values to model providers.
- Use different credentials for development and production.
- Limit credential scope and lifetime.

## 10. Observability and reporting

- Use structured, correlated logs with task and operation identifiers.
- Include timestamps, duration, status, module/provider, and safe error code.
- Redact secrets, cookies, authorization headers, and sensitive query parameters.
- Record model selection, quota errors, fallback decisions, and usage metadata when available.
- Keep task reports evidence-backed and identify untested areas.
- Avoid logging full source files or environment contents by default.

## 11. Documentation standards

- Keep a single authoritative source for each decision.
- Use stable requirement and architecture decision identifiers.
- Update documentation when an approved contract or behavior changes.
- Distinguish current implementation from proposed/future behavior.
- Include examples where they clarify a contract.
- Do not mark a feature complete based solely on documentation or generated code.

## 12. Change and release workflow

1. Link work to a requirement or approved issue.
2. Create a focused branch.
3. Inspect existing behavior and tests.
4. Implement the smallest complete change.
5. Run applicable checks and review diff.
6. Update documentation and evidence.
7. Open a pull request with risks and test outcomes.
8. Merge only after owner approval and required checks.
9. Preserve a way to identify/revert the change.

## 13. Phase discipline

- Complete and approve one phase before starting the next.
- Phase 0 is the frozen product foundation.
- Phase 1 formalizes the foundation in these documents.
- Phase 2 begins UI/UX design only after Phase 1 documentation is approved.
- Implementation must not start during this documentation phase.
- Scope changes must have a clear requirement or technical reason and owner approval.

## 14. Definition of done

A change is done only when:
- It meets referenced acceptance criteria.
- Relevant implementation and tests exist.
- Applicable tests have actually run and results are recorded.
- Diffs and side effects have been reviewed.
- Security and secret-handling requirements are respected.
- Documentation is updated where necessary.
- Remaining issues and untested behavior are disclosed.
