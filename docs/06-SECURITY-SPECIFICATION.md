# SDC Studio — Security Specification

**Status:** Draft  
**Objective:** Provide broad, low-friction engineering autonomy inside owner-authorized boundaries while protecting repositories, credentials, project data, and the host machine.

## 1. Security principles

1. Least privilege at integration boundaries; broad access only within explicitly selected and authorized project scopes.
2. Separate model reasoning from tool execution.
3. Treat model output, repository content, dependencies, and internet content as untrusted.
4. Keep secrets out of prompts, browser code, ordinary logs, and project repositories.
5. Execute project code outside the platform control plane.
6. Make mutations visible, attributable, and recoverable.
7. Use approval gates for high-impact operations, not repetitive ordinary development actions.
8. Respect provider, GitHub, hosting, operating-system, licensing, and legal controls.

Security controls must be proportionate and transparent. The target remains 10/10 user freedom for legitimate authorized engineering work; necessary security boundaries must not become arbitrary product restrictions.

## 2. Assets to protect

- GitHub App private key and installation tokens.
- AI provider API keys and local model configuration.
- User identity and session tokens.
- Private repositories and source code.
- Local workspace files and user data.
- Database and artifact storage.
- Environment variables and deployment secrets.
- Browser sessions, cookies, and authenticated preview data.
- Audit records, test evidence, and deployment logs.

## 3. Trust boundaries

| Boundary | Untrusted inputs | Required controls |
|---|---|---|
| Browser to API | User text, attachments, client state | Authentication, validation, size limits, authorization |
| Model provider | Prompts, context, model output | Data minimization, credential isolation, output validation |
| Repository to agent | Source files, comments, scripts, manifests | Treat as data; prompt-injection resistance; read-only discovery first |
| Web research | HTML, docs, code samples | Source validation, citation, no instruction authority |
| Agent to tools | Generated tool calls and arguments | Typed schemas, policy checks, scoped credentials, audit |
| Runner to host | Project commands and dependencies | Container isolation, resource limits, restricted mounts |
| Runner to network | Package downloads, APIs, callbacks | Explicit network policy, egress controls where feasible |
| Platform to artifacts | Logs, screenshots, traces | Secret redaction, project-scoped access, retention rules |

## 4. GitHub security

- Use a dedicated GitHub App.
- Install only on repositories explicitly selected by the owner.
- Request only permissions required by enabled functions.
- Use short-lived installation tokens and restrict them to selected repositories/permissions where supported.
- Never request or store the user's GitHub password.
- Avoid unrestricted personal access tokens as the default.
- Keep the App private key server-side or in protected local secret storage.
- Never send GitHub tokens/private keys to an AI model.
- Never print tokens into terminal output, logs, screenshots, or reports.
- Default branch is not a task execution target.
- Use task-specific branches/worktrees and record baseline SHA.
- Do not force-push or rewrite history.
- Do not change repository settings, rulesets, Actions permissions, or app installation scope through ordinary task tools.
- Stop and back off on GitHub rate limits and secondary rate-limit responses.
- Reconcile remote state after timeouts before retrying a mutation.

## 5. AI provider and model security

- Store API keys in protected server-side/local secret storage.
- Expose only configured provider/model choices to the UI.
- The model gateway is the only module allowed to use provider credentials.
- Do not include secrets or credentials in model context.
- Redact secret-like patterns from prompts and tool results before logging.
- Make provider data terms and free-tier data handling visible to the owner.
- Require owner acknowledgement before sending sensitive/private code to a provider whose data terms have not been accepted.
- Respect provider rate limits; use bounded retry and backoff.
- Do not rotate identities, proxy requests, or otherwise circumvent provider quotas or enforcement.
- Treat model output as untrusted proposals; validate all tool calls independently.

## 6. Prompt injection and untrusted content

Untrusted content includes repository README files, source comments, issue/PR text, commit messages, package scripts, web pages, uploaded documents, generated code, and terminal output.

Controls:
- Delimit and label retrieved content as untrusted data.
- Do not let repository or webpage instructions override platform policy, operator permissions, or tool schemas.
- Keep tool policy outside model-controlled content.
- Validate tool arguments against strict schemas.
- Resolve resource identifiers from authorized project state, not from model invention.
- Do not execute commands merely because an untrusted file says to execute them.
- When instructions conflict, follow platform policy and the authenticated operator's explicit request.

## 7. Execution isolation

- Project code runs in a container or equivalent isolated workspace, separate from the platform control plane.
- Use non-root execution where feasible.
- Apply CPU, memory, storage, process, and time limits.
- Mount only the selected project workspace and explicitly approved artifacts.
- Do not mount user home directories, SSH directories, browser profiles, platform database files, platform secrets, or Docker socket into project containers.
- Use scoped temporary credentials only when a task requires them.
- Keep environment variables task-scoped and redact values from output.
- Restrict network egress by default and allow documented destinations as required for package installation, testing, or user-authorized integration.
- Require approval for privileged host operations and actions outside the workspace boundary.
- Clean up stopped workspaces and temporary credentials according to retention policy.
- Treat package install hooks and build/test scripts as untrusted executable code.

## 8. Tool authorization and approval

Tool permissions are evaluated server-side and scoped to:
- Operator.
- Project.
- Repository/branch where applicable.
- Task and operation.
- Tool category and resource path.
- Execution mode.

Routine reversible operations inside an authorized workspace can proceed in Guided/Auto mode without repeated prompts.

Require explicit approval for:
- Production deployment.
- Deleting or overwriting user data outside task-created temporary output.
- Changing repository administration, security settings, rulesets, or App permissions.
- Creating, exposing, rotating, or modifying secrets.
- Force-push, history rewrite, or protected/default-branch merge.
- Privileged host commands or access outside the project workspace.
- Network or account actions with significant external impact.

An approval identifies the exact operation and target. It does not authorize unrelated actions.

## 9. Browser and network security

- Browser sessions run separately from the platform's authenticated browser profile.
- Do not reuse personal cookies or saved passwords.
- Restrict access to local/internal addresses from web-fetch and preview mechanisms to mitigate SSRF and cross-boundary access.
- Do not expose platform control endpoints or metadata services to project browser sessions.
- Redact authorization headers, cookies, tokens, and sensitive URL parameters.
- Keep screenshots and traces scoped to the project/task and protect them as potentially sensitive artifacts.
- Record actual browser events and HTTP outcomes; do not infer successful requests from page appearance alone.

## 10. Data protection

- Encrypt credentials at rest.
- Use TLS for remote service communication.
- Redact secrets and sensitive headers from logs and reports.
- Separate secret references from ordinary configuration.
- Limit artifact access by project/task authorization.
- Define retention and cleanup for logs, screenshots, traces, uploads, and temporary workspaces.
- Never commit secret-bearing environment files or generated credentials.
- Avoid transmitting private repository data to external model providers unless explicitly configured and accepted.

## 11. Audit and incident response

Record authentication/repository installation changes, provider/model configuration, tool requests and outcomes, approvals, file changes, commits, branches, PRs, command metadata, provider quota errors, browser diagnostics, test evidence, and deployment actions.

Logs must be redacted and access-controlled. For suspected credential exposure or unintended repository mutation:
1. Stop affected executions.
2. Revoke/rotate the impacted credential.
3. Identify task, operation, repository, branch, and affected artifacts.
4. Preserve relevant audit evidence.
5. Reconcile repository/workspace state.
6. Revert or recover changes through a reviewed operation.
7. Record root cause and corrective action.

## 12. Security verification requirements

Implementation must test:
- Cross-project authorization failures.
- Unauthorized repository access denial.
- Token/secret redaction in prompts, logs, and browser responses.
- Prompt-injection resistance for repository and web content.
- Workspace path traversal and mount boundary enforcement.
- Command timeout and resource limit enforcement.
- Container-to-control-plane isolation.
- Approval enforcement for high-impact actions.
- Rate-limit backoff and bounded retries.
- Artifact access controls.
- Recovery after runner/provider failures.

## 13. Security non-goals

SDC Studio is not intended to bypass authentication, authorization, paywalls, provider usage limits, GitHub enforcement, software licensing, or applicable law. It can perform broad authorized security analysis, debugging, and reverse engineering within the owner's rights and permitted environment.

## 14. Security change control

Any relaxation of an isolation boundary or approval requirement requires a documented threat/impact review and owner approval. The product must not weaken credential or repository safety merely to claim an unrestricted rating.
