# Isolate Environment-Sourced Secrets from Direct Process Access in Public API Modules: Configuration Abstractions Provide

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Public API modules require runtime configuration values such as domain identifiers, access tokens, and revalidation secrets to authenticate external service requests
- Direct inline access to process environment variables couples API contract implementations to runtime configuration sources, reducing testability and increasing exposure surface
- External client boundaries in public API modules handle sensitive credentials that must be validated before use, as evidenced by revalidation secret checks and header construction patterns
- Multiple modules across the codebase access overlapping environment variables for site metadata and service credentials, indicating a need for consistent secret handling patterns

## Problem Statement

Public API modules directly access process environment variables inline within function bodies and component definitions, embedding secret retrieval logic throughout the codebase. This pattern increases the risk of accidental logging, makes secret rotation difficult to audit, and complicates testing scenarios where environment state must be mocked or isolated.

## Decision

1. SHOULD: Configuration abstractions should provide type-safe interfaces that distinguish between required secrets, optional configuration, and public metadata.

## Policy Block

- SHOULD Configuration abstractions should provide type-safe interfaces that distinguish between required secrets, optional configuration, and public metadata.

In scope:
- Modules that export public API contracts for external service integration
- Components that render metadata derived from environment configuration
- Request handlers that validate webhook signatures or revalidation tokens
- Utility functions that construct base URLs or service endpoints from environment state

Out of scope:
- Internal utility modules that do not expose public API contracts
- Build-time configuration scripts that run outside the application runtime
- Development tooling that requires direct environment access for debugging

Exceptions:
- EXC-001: A module performs environment variable validation as its sole responsibility and exports only validation results, not the raw secret values

## Rationale

- The evidence shows four modules directly accessing process environment variables for secrets including store domains, access tokens, and revalidation secrets, creating multiple points of exposure
- Inline secret access in API contract implementations couples authentication logic to runtime configuration sources, making it difficult to test API behavior independently of environment state
- The pattern of validating revalidation secrets with console error logging demonstrates the risk of accidental secret exposure when validation logic is distributed across modules
- Centralizing secret retrieval through a configuration abstraction enables consistent validation, audit logging, and rotation procedures across all public API modules

## Consequences

Positive:
- Reduced risk of accidental secret exposure through logging or error messages
- Improved testability of API modules through dependency injection of configuration values
- Centralized audit trail for secret access patterns and validation failures
- Simplified secret rotation procedures with a single configuration layer to update

Negative:
- Additional abstraction layer increases initial implementation complexity
- Configuration module becomes a critical dependency for all public API modules
- Potential performance overhead from configuration validation at initialization time
- Requires refactoring existing modules that currently access environment variables directly

## Alternatives

- Continue direct process environment variable access throughout API modules (rejected)
  Rejected because: Maintains current exposure risk, couples API contracts to runtime configuration sources, and provides no centralized validation or audit capability
  When valid: Never recommended for production systems handling external API credentials
- Use a third-party secrets management service with runtime credential fetching (deferred)
  Rejected because: Introduces external service dependency and network latency for credential retrieval; may be considered for future enhancement after establishing configuration abstraction pattern
  When valid: When secret rotation frequency or compliance requirements justify the operational complexity of external secrets management
- Implement per-module configuration objects with local validation (rejected)
  Rejected because: Duplicates validation logic across modules, creates inconsistent error handling patterns, and provides no centralized audit capability
  When valid: Only when modules have completely distinct configuration requirements with no overlapping secret values

## Risks

- Configuration abstraction layer becomes a single point of failure if initialization errors are not handled gracefully
  Mitigation: Implement fail-fast validation at application startup with clear error messages identifying missing or invalid configuration keys
  Owner: Engineering team
- Cached secret values in long-running processes may become stale if external rotation occurs without process restart
  Mitigation: Document secret rotation procedures requiring process restart, or implement time-based cache invalidation for environments with frequent rotation requirements
  Owner: Engineering team
- Incomplete migration leaves some modules using direct environment access while others use configuration abstraction
  Mitigation: Implement static analysis rules to detect direct process environment access in API modules and enforce through continuous integration checks
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Create a configuration module that exports typed interfaces for each category of secret: service credentials, domain identifiers, and validation tokens. Initialize and validate all required secrets at module load time.
- Refactor existing API modules to accept configuration objects as constructor parameters or function arguments rather than accessing process environment directly. Prioritize modules that construct external client requests with authentication headers.
- Implement structured error types for configuration validation failures that identify the missing key without exposing secret values. Ensure error messages are safe to log and display in development environments.

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the linting rules that detect direct process environment access patterns in modules under the public API scope
- Locate the project's test suite configuration and run integration tests that verify API modules function correctly with injected configuration objects in isolated test environments
- Identify the project's type checking tooling and verify that configuration interfaces enforce required secret properties at compile time

Accept when:
- Static analysis reports zero instances of direct process environment access in modules that export public API contracts
- Integration tests pass with mocked configuration objects, demonstrating API modules do not depend on runtime environment state
- Type checking confirms all API modules receive configuration through typed interfaces with required secret properties

## Enforcement

- Verified by: Static analysis rules executed in continuous integration pipeline
- Verified by: Code review checklist requiring configuration abstraction for new API modules
- Verified by: Integration test coverage requirements for API modules with injected configuration
- Violation handling: Continuous integration pipeline fails on detection of direct process environment access in API modules
- Violation handling: Code review blocks merge requests that introduce new direct environment access patterns
- Violation handling: Automated alerts notify architecture team of violations detected in production monitoring
- Exception process: Submit exception request to architecture review board with justification for direct environment access
- Exception process: Document the specific module, secret type, and rationale for exception in architecture decision log
- Exception process: Implement compensating controls such as enhanced audit logging or secret masking for approved exceptions