# Isolate External API Secrets in Environment Variables with Runtime Validation: Client Functions Shall

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Internal API modules coordinate with external service providers requiring authentication credentials that must not be embedded in source code or version control.
- The runtime environment provides configuration through environment variables that are read at module initialization or request time.
- Secret validation occurs at the boundary where external HTTP requests are constructed, ensuring credentials are present before network calls are made.
- The pattern separates configuration sourcing from business logic, allowing credentials to be rotated without code changes.

## Problem Statement

Internal API modules must authenticate with external service providers while preventing credential exposure in source control, ensuring runtime availability of secrets, and validating their presence before making authenticated requests.

## Decision

1. MUST: API client functions SHALL validate the presence of required credentials before constructing authenticated HTTP requests and SHALL reject operations with descriptive error messages when credentials are missing.

## Policy Block

- MUST API client functions SHALL validate the presence of required credentials before constructing authenticated HTTP requests and SHALL reject operations with descriptive error messages when credentials are missing.

In scope:
- All internal API modules that authenticate with external HTTP services
- Webhook handlers that validate incoming requests using shared secrets
- Client libraries that construct authenticated requests to third-party APIs
- Configuration modules that provide credentials to API clients

Out of scope:
- Public API endpoints exposed to external consumers
- Database connection strings managed by ORM or database client libraries
- TLS certificates and private keys managed by the runtime or reverse proxy
- Session tokens or JWTs generated and validated within the application

Exceptions:
- EXC-001: Development and testing environments where credential security is not required

## Rationale

- The evidence shows environment variable access patterns for store domain, access token, and revalidation secret, demonstrating separation of credentials from code.
- Validation logic that logs invalid secret errors without exposing values indicates defensive programming at authentication boundaries.
- The pattern enables credential rotation through environment reconfiguration without requiring code deployment or version control changes.
- Sourcing secrets from the runtime environment aligns with twelve-factor application principles and container orchestration best practices.

## Consequences

Positive:
- Credentials can be rotated or updated through environment reconfiguration without modifying source code or redeploying application logic.
- Secret values are excluded from version control history, reducing the risk of credential exposure through repository access.
- Runtime validation at request boundaries provides early failure detection when credentials are misconfigured or missing.
- The pattern supports multiple deployment environments with different credentials using the same codebase.

Negative:
- Environment variable management adds operational complexity, requiring secure storage and injection mechanisms in deployment pipelines.
- Missing or misconfigured environment variables may not be detected until runtime, potentially causing production failures if validation is deferred.
- Debugging authentication issues requires access to environment configuration, which may be restricted in production environments.
- The pattern does not address secret rotation during runtime without process restart or environment reload.

## Alternatives

- Store credentials in configuration files deployed alongside application code (rejected)
  Rejected because: Configuration files are typically committed to version control or included in deployment artifacts, increasing the risk of credential exposure and making rotation more complex
  When valid: In environments with file-based secret management systems that inject credentials into configuration files at deployment time with appropriate access controls
- Retrieve credentials from a dedicated secrets management service at runtime (deferred)
  Rejected because: Adds dependency on external secrets service and increases complexity, though provides better audit trails and dynamic rotation capabilities
  When valid: In large-scale deployments requiring centralized secret management, audit logging, and runtime credential rotation without process restart
- Use OAuth token exchange or service account impersonation for credential-less authentication (rejected)
  Rejected because: Requires external service support for delegated authentication mechanisms, which may not be available for all third-party APIs
  When valid: When integrating with services that support OAuth 2.0, OpenID Connect, or cloud provider identity federation

## Risks

- Environment variables may be logged or exposed through process inspection tools, error messages, or debugging interfaces
  Mitigation: Implement logging filters to redact secret values, restrict process inspection in production, and validate that error messages do not include credential values
  Owner: Engineering team
- Missing environment variables may not be detected until runtime request handling, causing production failures
  Mitigation: Implement startup validation that checks for required environment variables and fails fast with clear error messages before accepting traffic
  Owner: Engineering team
- Credential rotation requires coordinated environment updates across all running instances, creating a window for authentication failures
  Mitigation: Implement graceful credential transition periods where both old and new credentials are accepted, or use rolling deployment strategies that update environment variables incrementally
  Owner: Operations team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Create environment variable validation functions that check for required credentials at module initialization and provide clear error messages indicating which variables are missing and where they should be configured.
- Implement logging filters or sanitization functions that detect and redact secret patterns in error messages and debug output, ensuring validation failures log only the fact of failure without exposing credential values.
- Document required environment variables in deployment configuration templates and provide example values that clearly indicate they are placeholders requiring replacement in production environments.

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify environment variable access patterns in internal API modules
- Locate the project's test suite and execute tests that validate credential validation logic and error handling for missing environment variables
- Inspect the project's deployment configuration templates to confirm required environment variables are documented and no credential values are committed to version control

Accept when:
- All internal API modules source external service credentials exclusively from environment variables without hardcoded values in source code
- Validation logic rejects operations with missing credentials and logs failures without exposing secret values in error messages or standard output
- Deployment configuration documents all required environment variables and version control history contains no committed credential values

## Enforcement

- Verified by: Code review checklist requiring verification that no credential literals appear in source code and all external service authentication uses environment variable sourcing
- Verified by: Static analysis scanning for hardcoded credential patterns and environment variable access compliance
- Verified by: Deployment pipeline validation that checks for required environment variables before promoting builds to production
- Violation handling: Pull requests containing hardcoded credentials are blocked from merge and flagged for immediate remediation
- Violation handling: Static analysis violations trigger build failures and require explicit exception approval with documented justification
- Violation handling: Production deployment failures due to missing environment variables halt the deployment process and trigger incident response procedures
- Exception process: Request exception approval from engineering team lead with documented justification for development or testing scenarios
- Exception process: Document approved exceptions in environment configuration templates with clear markers indicating non-production use
- Exception process: Review exceptions quarterly to ensure they remain valid and have not been promoted to production environments