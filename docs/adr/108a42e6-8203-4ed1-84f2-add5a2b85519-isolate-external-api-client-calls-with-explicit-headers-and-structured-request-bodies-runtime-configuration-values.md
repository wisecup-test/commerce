# Isolate External API Client Calls with Explicit Headers and Structured Request Bodies: Runtime Configuration Values

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase integrates with an external storefront API using runtime configuration sourced from environment variables for domain, access token, and revalidation secret.
- HTTP client calls are constructed with explicit headers including content type and authentication tokens, and request bodies are serialized as JSON with conditional query and variables fields.
- Server-side runtime boundaries are accessed through async cookie and header accessors, indicating a framework that separates request context from business logic.
- Error handling for invalid secrets is logged to console, suggesting a validation layer for webhook or revalidation endpoints.
- Public API contracts expose cart operations (create, add, remove, update) alongside a generic fetch function, establishing a boundary between internal implementation and external consumers.

## Problem Statement

Internal APIs that integrate with external services require a consistent pattern for constructing authenticated HTTP requests, managing runtime configuration, and isolating client boundaries from business logic to ensure maintainability, security, and testability.

## Decision

1. MUST: Runtime configuration values MUST be validated before use, with invalid values logged and rejected.

## Policy Block

- MUST Runtime configuration values MUST be validated before use, with invalid values logged and rejected.

In scope:
- All HTTP client calls to external storefront or webhook APIs
- Functions that construct authenticated requests using environment-sourced credentials
- Public API contracts that expose cart or resource operations to internal consumers
- Runtime configuration validation for secrets and tokens

Out of scope:
- Internal service-to-service calls within the same deployment boundary
- Client-side browser API calls that do not require server-side authentication
- Static configuration or build-time constants
- Database or cache layer access patterns

## Rationale

- The evidence shows a consistent pattern of fetch calls with explicit headers, JSON body serialization, and environment-based authentication, indicating an established boundary between internal API logic and external service integration.
- Separating runtime configuration from client logic enables secure credential management, easier testing through dependency injection, and clearer ownership boundaries between infrastructure and application code.
- Exposing domain operations through named functions (createCart, addToCart, etc.) rather than raw fetch calls improves discoverability, type safety, and encapsulation of protocol details.
- The presence of async cookie and header accessors alongside validation logic demonstrates a framework-aware approach that respects request lifecycle boundaries and security requirements.

## Consequences

Positive:
- Clear separation between external API client boundaries and internal business logic improves testability and maintainability.
- Explicit header construction and credential injection from runtime configuration reduces risk of credential leakage and improves security posture.
- Named domain operation functions provide a stable internal API contract that shields consumers from protocol changes.
- Conditional JSON serialization with query and variables fields enables flexible request construction while maintaining type safety.

Negative:
- Additional abstraction layers between raw HTTP calls and business logic may increase cognitive overhead for developers unfamiliar with the pattern.
- Runtime configuration validation adds execution overhead and requires consistent error handling across all client functions.
- Framework-specific async accessors for cookies and headers create coupling to the server-side runtime environment, limiting portability.

## Alternatives

- Use a generated SDK or client library provided by the external service vendor instead of constructing raw fetch calls. (rejected)
  Rejected because: The evidence shows explicit fetch construction with custom header management and conditional body serialization, indicating a deliberate choice to maintain control over request formation and avoid vendor SDK dependencies.
  When valid: When the external service provides a well-maintained, type-safe SDK that matches the project's runtime environment and does not introduce unacceptable dependency weight or version conflicts.
- Inline all HTTP client logic directly in route handlers or business logic functions without abstraction. (rejected)
  Rejected because: The evidence demonstrates clear separation through named functions (shopifyFetch, createCart, etc.) and public API contracts, indicating a deliberate architectural boundary that improves testability and reusability.
  When valid: For prototype or single-use integrations where the overhead of abstraction exceeds the benefit, or when the integration is guaranteed to remain isolated to a single call site.
- Store authentication credentials in a centralized secrets manager with runtime resolution instead of environment variables. (deferred)
  Rejected because: Not applicable - this is an infrastructure decision orthogonal to the client boundary pattern.
  When valid: When security requirements mandate centralized secret rotation, audit logging, or multi-environment credential isolation beyond what environment variables provide.

## Risks

- Environment variable misconfiguration or missing credentials will cause runtime failures that may not be caught until deployment.
  Mitigation: Implement startup validation that checks for required environment variables and fails fast with clear error messages. Add integration tests that verify credential configuration in staging environments.
  Owner: Engineering team
- Changes to external API authentication schemes or request formats will require updates across multiple client functions.
  Mitigation: Centralize request construction logic in a single base function that all domain operations call, ensuring changes to headers or serialization only require updates in one location.
  Owner: Engineering team
- Console-based error logging for invalid secrets may not provide sufficient visibility in production environments.
  Mitigation: Integrate structured logging with appropriate severity levels and ensure logs are aggregated to a centralized monitoring system with alerting for authentication failures.
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
- Locate the base HTTP client function that constructs fetch calls with headers and body serialization. Ensure all domain operation functions delegate to this base function rather than duplicating request construction logic.
- Identify the runtime configuration module that sources environment variables. Verify that all required credentials are validated at startup or first use, with clear error messages for missing or invalid values.
- Review the public API contract exports to ensure domain operations are exposed as named functions with clear type signatures, hiding protocol details from consumers.
- Examine the framework's async accessor patterns for cookies and headers. Ensure these are consistently used across all functions that require request context, maintaining separation from business logic.

## Continuation Context


Verify commands:
- Locate the project's test suite directory and identify integration tests that verify external API client calls with mocked responses.
- Discover the project's static analysis or linting configuration and run the configured checks to verify header construction and credential handling patterns.
- Identify the project's environment variable validation logic and execute the startup validation to confirm all required credentials are checked.

Accept when:
- All external API client functions construct requests with explicit headers, authentication tokens from runtime configuration, and conditional JSON body serialization.
- Public API contracts expose domain operations as named functions that encapsulate client implementation details and hide protocol specifics.
- Runtime configuration validation rejects invalid or missing credentials with logged errors before attempting external API calls.

## Enforcement

- Verified by: Code review checklist verifying that new external API integrations follow the established client boundary pattern with explicit headers and runtime configuration.
- Verified by: Integration tests that mock external API responses and verify request construction, header injection, and error handling.
- Verified by: Static analysis rules that flag hardcoded credentials or inline fetch calls that bypass the established client abstraction layer.
- Violation handling: Pull requests that introduce external API calls without following the established pattern are blocked until refactored to use the base client function and runtime configuration.
- Violation handling: Hardcoded credentials or authentication tokens trigger immediate security review and remediation before merge.
- Violation handling: Integration test failures for client boundary validation block deployment to staging and production environments.
- Exception process: Exceptions for alternative client patterns require architectural review and documented justification in the pull request description.
- Exception process: Temporary inline implementations for prototyping must include a tracking issue for refactoring to the standard pattern before production deployment.
- Exception process: External service integrations that require vendor-specific SDKs must demonstrate that the SDK provides equivalent security, testability, and maintainability benefits.