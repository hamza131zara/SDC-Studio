# SDC Studio — External Integration Strategy

**Status:** Draft  
**Purpose:** Specify how the product connects external AI providers, GitHub, public research sources, and hosting providers without coupling the core platform to one vendor.

## 1. Integration principles

- Every external provider is accessed through a dedicated adapter.
- Credentials remain outside model context and client-side application code.
- The owner explicitly selects integrations and scopes permissions.
- Provider capabilities, limits, errors, and data terms are visible.
- Retries are bounded and respect rate limits.
- External integration failures must not corrupt task state or produce false success.
- The product's core orchestration contract must not depend on one vendor's API shape.
- Candidate providers and their terms are verified when the corresponding integration phase begins; this document does not promise free-tier availability or fixed quotas.

## 2. AI provider integration

### 2.1 Provider gateway responsibilities

- Store provider configuration and protected credential reference.
- Discover or configure supported model identifiers.
- Normalize chat/generation requests.
- Normalize streaming events where supported.
- Record model capabilities: text, image, tool/function calling, structured output, streaming, and context limit when documented.
- Preserve provider-specific metadata and errors.
- Track usage/token/credit information only where provider exposes it.
- Detect quota and rate-limit conditions.
- Apply bounded retry/backoff and optional fallback.
- Let the owner override automatic routing.

### 2.2 Candidate provider categories

The initial provider evaluation may include:
- Google Gemini API.
- Groq inference API.
- OpenRouter model gateway.
- Cloudflare Workers AI where applicable.
- Ollama for local inference.

This is a candidate list from the product planning baseline, not a guarantee that all services will provide suitable free access or support all required features at implementation time. Recheck current model availability, free-tier limits, data handling, commercial-use terms, geography, and tool-calling support during Phase 4.

### 2.3 Model selection strategy

Routing may consider:
- Task type and required capabilities.
- User-selected model preference.
- Context window and supported modalities.
- Structured output/tool calling support.
- Current configured availability.
- Latency and observed reliability.
- Free-tier quota and owner budget.
- Provider data terms and project sensitivity.

User selection takes precedence unless the model is unavailable or lacks the requested capability. In that case explain the limitation and offer another configured model. Do not silently switch to a materially different provider when data terms or privacy expectations differ.

### 2.4 Free-tier and quota behavior

- Free tiers are treated as limited, changeable offers.
- Show configured provider and model status.
- Record quota/rate-limit responses.
- Respect Retry-After and provider documentation.
- Use bounded exponential backoff with jitter where appropriate.
- Do not create alternate identities, rotate accounts, proxy requests, or otherwise evade quotas.
- If all configured providers are unavailable, preserve task state and explain how to resume.
- Offer local inference only if the owner has configured it and the task is compatible.

### 2.5 Data handling

Before sending private repository context:
- Identify which provider receives the data.
- Display/record applicable data-handling acknowledgement.
- Exclude secrets, tokens, credentials, and unnecessary private files.
- Allow owner-defined excluded paths and sensitive-file patterns.
- Keep prompts/logs redacted where possible.
- Do not claim that an API is private/no-training unless current provider terms support that claim.

## 3. GitHub integration

### 3.1 Integration type

Use a dedicated GitHub App for the SDC Studio product. The existing ChatGPT GitHub connection used during repository documentation is separate and does not become the product's GitHub integration.

### 3.2 Installation and repository selection

1. Register the SDC Studio GitHub App.
2. Request the minimum repository permissions required by enabled features.
3. Operator installs it on selected repositories only.
4. Record installation ID, owner, selected repository IDs, and granted permission snapshot.
5. List repositories only through authorized installations.
6. Allow operator to disconnect/uninstall the integration.
7. On uninstall/revocation, stop operations and mark the repository connection unavailable.

### 3.3 Initial permission baseline

Proposed least-privilege permissions to validate against actual functionality:
- Metadata: read.
- Contents: read/write when implementation reaches repository mutation.
- Pull requests: read/write when PR workflows are enabled.
- Issues: read/write only if issue workflow is implemented.
- Actions/checks: read if CI status is displayed.
- Workflows: no access unless a later approved requirement needs workflow-file updates.
- Administration: no access in normal project operation.
- Secrets: no access in ordinary repository tasks.

Do not request permissions merely because they are available. Add them only when a requirement and implementation need them.

### 3.4 Token and API handling

- Use short-lived GitHub App installation tokens.
- Restrict tokens to selected repositories and permissions where supported.
- Keep private key in protected server-side or local secret storage.
- Do not send tokens/private key to the AI model.
- Respect primary/secondary rate limits and abuse detection.
- Use bounded backoff and stop repeated requests after a throttle response.
- Reconcile remote state after timeouts and uncertain mutation outcomes.

### 3.5 Repository workflow

- Read metadata and baseline before work.
- Perform read-only discovery at connection time.
- Use isolated task branch/worktree.
- Show exact diffs.
- Commit with clear attribution.
- Open a PR for review.
- Do not directly write the default branch or force-push by default.
- Preserve existing user work and stop on unexpected branch movement.

## 4. Public research integration

### 4.1 Purpose

Research supports technical questions, official documentation retrieval, API/library compatibility, debugging, and deployment option comparison.

### 4.2 Requirements

- Prefer official vendor documentation and release notes for technical behavior.
- Capture source URL, title, publisher/domain, date when available, and retrieval time.
- Distinguish current facts from general technical reasoning.
- Cite external claims in user-facing answers/reports.
- Respect website access controls and rate limits.
- Treat webpage text, code snippets, and embedded instructions as untrusted data.
- Do not submit credentials or private repository content to public websites.
- Do not use scraping or access methods that bypass restrictions.

## 5. Hosting/deployment integration

### 5.1 Provider-neutral interface

A hosting adapter should support applicable operations:
- Inspect target capabilities.
- Validate project compatibility.
- Prepare deployment configuration.
- Configure protected environment-variable references.
- Start deployment after approval.
- Retrieve deployment status and logs.
- Return deployed URL and deployment identity.
- Run post-deployment health checks.
- Roll back/redeploy when supported.

Not every provider supports every operation. Capability metadata must show what is actually supported.

### 5.2 Candidate providers

Cloudflare Pages/Workers is the initial candidate for compatible static and serverless applications. Other providers such as Netlify or Vercel may be evaluated when their current free/commercial terms and project compatibility are appropriate.

Provider-specific limitations, free-tier quotas, commercial-use restrictions, runtime limits, build limits, data region, and custom-domain behavior must be checked at implementation time. Never assume that a free static hosting plan can run a persistent backend, arbitrary container, database, or background worker.

### 5.3 Project URL and custom domain

Potential URL options:
- Provider-issued project subdomain.
- A platform-owned base domain with project subdomains, once the owner controls a domain and configures DNS/TLS/wildcard routing.
- Customer-owned custom domain, where the owner/customer controls the domain and DNS records.

SDC Studio cannot issue arbitrary public DNS names without domain ownership/control. The deployment UI must explain who owns the domain and who pays for it.

### 5.4 Deployment safety

- Separate preview/staging/production targets where provider supports them.
- Never place production secrets in repository files.
- Require explicit approval before production deployment.
- Verify build artifacts and target compatibility before deployment.
- Persist provider deployment ID, URL, status and logs.
- Confirm health/availability after deployment.
- Preserve prior deployment metadata for rollback where supported.
- If a provider cannot safely roll back, explain the recovery procedure before deployment.

## 6. Integration error taxonomy

Normalize at least:
- Authentication failure.
- Permission denied.
- Resource not found.
- Validation error.
- Rate limited.
- Quota exhausted.
- Provider unavailable.
- Timeout.
- Conflict/stale revision.
- Unsupported capability.
- Policy/data-term acknowledgement required.
- Deployment/build failure.
- Unknown external error.

Each error has source, safe message, retryability, operation ID, redacted details, and retry-after metadata when available.

## 7. Integration acceptance

Each adapter must pass:
- Authentication success/failure handling.
- Permission-scope enforcement.
- Secret redaction.
- Request/response contract tests.
- Timeout and retry tests.
- Rate-limit/quota behavior.
- Provider unavailable behavior.
- Remote state reconciliation after uncertain timeouts.
- Audit event generation.
- Disconnect/revocation handling.

## 8. Phase ownership

| Integration | Main implementation phase |
|---|---|
| AI cloud provider adapters | Phase 4 |
| Local Ollama adapter | Phase 4 |
| GitHub App and repository API | Phase 5 |
| Research adapter | Phase 4/5 according to orchestration need |
| Project execution adapter | Phase 6 |
| Browser/preview integration | Phase 7 |
| Hosting/deployment adapters | Phase 8 |

This document records integration boundaries only. Actual provider registration, credentials, and live integration work begin in their assigned phases.
