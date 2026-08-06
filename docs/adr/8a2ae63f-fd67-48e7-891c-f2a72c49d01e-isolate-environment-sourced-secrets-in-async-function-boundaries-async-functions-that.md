# Isolate Environment-Sourced Secrets in Async Function Boundaries: Async Functions That

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all code that accesses environment-sourced configuration or secrets within async function boundaries.

## Context

- The codebase accesses environment-sourced secrets through process environment variables within async function boundaries that coordinate external API calls and server-side rendering operations.
- Multiple async functions retrieve secrets at runtime and pass them as headers to external HTTP endpoints, creating a pattern where secret handling is coupled to concurrency boundaries.
- The pattern appears in both API integration modules and server-side rendering components, indicating a cross-cutting concern for how secrets flow through async operations.
- Runtime configuration sources are read directly from the process environment at function invocation time rather than being injected or centralized, creating multiple points of access.

## Problem Statement

When async functions directly access environment-sourced secrets at multiple invocation points without centralized validation or isolation, the system lacks a consistent boundary for secret lifecycle management, audit trails, and error handling. This creates risk of secret leakage through logging, inconsistent validation across call sites, and difficulty tracing secret usage patterns.

## Decision

1. MUST: Async functions that coordinate external API calls using secrets MUST isolate secret retrieval from business logic and treat secrets as opaque values after validation.

## Policy Block

- MUST Async functions that coordinate external API calls using secrets MUST isolate secret retrieval from business logic and treat secrets as opaque values after validation.

In scope:
- All async functions that retrieve configuration or secrets from process environment variables
- Functions that construct HTTP headers or request bodies containing authentication credentials
- Server-side rendering components that access environment-sourced configuration
- API integration modules that coordinate external service calls using secrets

Out of scope:
- Client-side code that does not have access to server environment variables
- Static configuration values that are not secrets
- Build-time environment variable substitution
- Development-only configuration that does not reach production

Exceptions:
- EXC-001: Non-secret environment variables such as feature flags or public site names may be accessed directly without validation isolation
- EXC-002: Webhook validation endpoints may log secret comparison failures for security monitoring purposes, provided the actual secret value is never included in logs

## Rationale

- The evidence shows environment-sourced secrets being accessed within async function boundaries that coordinate external API calls, creating a pattern where secret handling is distributed across multiple call sites rather than centralized.
- Observed logging patterns include error messages that could inadvertently expose secret validation failures, indicating a need for explicit rules about secret visibility in observability outputs.
- The pattern of reading secrets at function invocation time within async boundaries suggests that secret lifecycle is tied to concurrency model, requiring isolation rules to prevent leakage across async contexts.
- With 88.15% confidence across 2 files, the pattern is significant enough to warrant standardization but limited enough in scope to allow focused enforcement.

## Consequences

Positive:
- Centralized secret validation reduces the risk of inconsistent error handling and makes it easier to audit secret access patterns across the codebase.
- Explicit isolation of secret retrieval from business logic creates clear boundaries for security review and testing.
- Preventing secret values from appearing in logs or error messages reduces the attack surface for credential leakage.
- Standardizing secret handling within async function boundaries makes it easier to migrate to more sophisticated secret management systems in the future.

Negative:
- Centralizing secret validation adds an additional abstraction layer that may increase initial implementation complexity.
- Strict isolation rules may require refactoring existing async functions that currently access environment variables directly.
- Caching validated secrets within request contexts adds memory overhead and requires careful lifecycle management to prevent stale references.
- The pattern may create friction during local development if secret validation is too strict or error messages are too opaque.

## Alternatives

- Allow direct environment variable access throughout async functions without centralized validation or isolation (rejected)
  Rejected because: This approach creates multiple points of failure for secret validation, makes audit trails difficult to maintain, and increases risk of secret leakage through inconsistent error handling across call sites.
  When valid: Only appropriate for non-production development environments where secret leakage has no security impact
- Inject all secrets as function parameters rather than reading from environment at invocation time (deferred)
  Rejected because: While this would create clearer boundaries, it requires significant refactoring of existing async function signatures and may not be compatible with framework conventions for server-side rendering.
  When valid: Consider for new modules or when refactoring existing code; provides better testability and explicit dependency tracking
- Use a dedicated secret management service with runtime credential rotation instead of environment variables (deferred)
  Rejected because: This represents a more sophisticated approach but requires infrastructure changes and may introduce latency in async function execution. The current pattern can serve as a stepping stone.
  When valid: Appropriate for production systems with high security requirements or when secret rotation policies demand it

## Risks

- Existing async functions may inadvertently log secret values during error conditions, creating credential leakage in observability systems.
  Mitigation: Audit all error logging statements in functions that access environment-sourced secrets and implement secret redaction in logging infrastructure.
  Owner: Security team with engineering support
- Centralized validation logic may become a single point of failure if not properly tested or if it introduces performance bottlenecks in async function execution.
  Mitigation: Implement comprehensive unit tests for secret validation logic and monitor performance impact on async function latency. Consider caching validated secrets within request scope.
  Owner: Engineering team
- Developers may bypass isolation rules during rapid development or when debugging, creating inconsistent secret handling patterns.
  Mitigation: Implement automated checks in continuous integration to detect direct environment variable access in async functions and provide clear documentation on approved patterns.
  Owner: Engineering team with DevOps support

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Create a centralized secret validation module that exports typed functions for retrieving and validating each required secret. This module should be the single source of truth for environment variable names and validation rules.
- Implement a request-scoped context or container that holds validated secrets for the duration of a single request lifecycle. This prevents repeated environment access while ensuring secrets do not leak across request boundaries.
- Add linting rules or static analysis checks to detect direct process environment access in async functions outside the approved validation module. This provides automated enforcement of the isolation pattern.
- Document the classification of each environment variable as either secret or non-secret configuration. Non-secret values may be accessed directly, but the classification must be explicit and reviewed.

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the linting rules that detect direct environment variable access patterns in async function bodies.
- Locate the project's test suite and run integration tests that verify secret validation behavior, including tests for missing secrets, invalid formats, and proper error message redaction.
- Identify the project's security scanning tools and execute checks that detect potential secret leakage in logging statements, error messages, and serialized responses.

Accept when:
- All async functions that use environment-sourced secrets retrieve them through the centralized validation module rather than direct environment access.
- Static analysis or linting passes without detecting direct process environment access in async function bodies outside approved patterns.
- Integration tests confirm that missing or invalid secrets produce non-revealing error messages and that secret values never appear in logs or error responses.
- Security scanning tools report no instances of potential secret leakage in observability outputs or client-visible responses.

## Enforcement

- Verified by: Automated static analysis in continuous integration pipeline that detects direct environment variable access patterns
- Verified by: Code review checklist requiring verification that new async functions use centralized secret validation
- Verified by: Security scanning tools that detect potential secret leakage in logs and error messages
- Verified by: Integration test suite that validates secret handling behavior across all async function boundaries
- Violation handling: Continuous integration pipeline fails if static analysis detects unapproved direct environment access patterns
- Violation handling: Code review blocks merge until secret handling follows approved isolation patterns
- Violation handling: Security scanning alerts trigger immediate review and remediation of potential secret leakage
- Violation handling: Violations discovered in production code are treated as security incidents requiring hotfix deployment
- Exception process: Developer documents the specific reason why centralized validation cannot be used for a particular async function
- Exception process: Security team reviews the exception request and assesses risk of the alternative approach
- Exception process: If approved, the exception is documented in code comments with reference to security review ticket
- Exception process: Exceptions are reviewed quarterly to determine if they can be eliminated through refactoring