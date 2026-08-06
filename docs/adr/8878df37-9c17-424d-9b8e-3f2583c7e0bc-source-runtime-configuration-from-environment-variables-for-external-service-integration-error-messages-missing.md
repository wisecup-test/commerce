# Source Runtime Configuration from Environment Variables for External Service Integration: Error Messages Missing

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The internal API layer integrates with an external storefront service requiring authentication credentials and endpoint configuration at runtime
- Configuration values for domain, access token, and revalidation secret must be available to multiple API functions without hardcoding
- The runtime environment provides a process-level configuration source that can be accessed synchronously during module initialization and request handling
- API functions perform authenticated HTTP requests to external endpoints and validate incoming webhook requests using shared configuration values

## Problem Statement

Internal API modules that integrate with external services require runtime configuration for endpoints, credentials, and secrets. Hardcoding these values creates security risks and deployment inflexibility. The system needs a consistent, discoverable mechanism to source configuration at runtime that supports both synchronous initialization and request-scoped validation.

## Decision

1. SHOULD: Error messages for missing configuration SHOULD identify the specific configuration key without exposing the value

## Policy Block

- SHOULD Error messages for missing configuration SHOULD identify the specific configuration key without exposing the value

In scope:
- Internal API modules that make authenticated HTTP requests to external services
- Webhook handlers that validate incoming requests using shared secrets
- Configuration initialization code that prepares API client instances
- Error handling logic that reports configuration issues

Out of scope:
- Public API contracts exposed to external consumers
- Client-side code that does not have access to server-side environment
- Static configuration that does not vary between deployment environments
- Configuration for internal services that do not require authentication

## Rationale

- The evidence shows three distinct configuration keys sourced from process environment variables, all following a service-prefixed naming pattern that enables clear identification and namespace isolation
- Multiple API functions reference the same configuration source, demonstrating a shared configuration mechanism that avoids duplication and maintains consistency across the internal API surface
- Webhook validation logic retrieves the revalidation secret from the same configuration source, establishing a unified pattern for both outbound authentication and inbound request validation
- The pattern supports deployment flexibility by externalizing environment-specific values while maintaining type safety and compile-time verification of configuration key names

## Consequences

Positive:
- Configuration changes do not require code modifications or recompilation, enabling rapid deployment across environments
- Credentials and secrets remain external to source control, reducing security exposure and simplifying credential rotation
- Multiple API functions share a single source of truth for configuration, eliminating inconsistencies and reducing maintenance burden
- The pattern integrates naturally with container orchestration and deployment platforms that inject environment variables

Negative:
- Runtime configuration errors are discovered at deployment or request time rather than compile time, potentially causing production failures
- Environment variable management becomes a deployment dependency that must be coordinated across all environments
- Missing or misconfigured environment variables can cause silent failures or unclear error messages if validation is insufficient
- Configuration values are process-global, making it difficult to support multi-tenancy or per-request configuration overrides

## Alternatives

- Hardcode configuration values directly in source code (rejected)
  Rejected because: Hardcoding credentials creates security vulnerabilities, prevents environment-specific deployment, and requires code changes for configuration updates
  When valid: Only acceptable for truly static values that never vary across environments and contain no sensitive data
- Use a configuration file loaded from the filesystem at runtime (rejected)
  Rejected because: File-based configuration adds filesystem dependencies, complicates container deployments, and requires additional file management infrastructure compared to environment variables
  When valid: Appropriate for complex structured configuration that exceeds the simplicity of key-value pairs or requires hierarchical organization
- Retrieve configuration from a remote configuration service at runtime (rejected)
  Rejected because: Remote configuration introduces network dependencies, latency, and additional failure modes for a simple set of static deployment-time values
  When valid: Justified for dynamic configuration that changes during runtime or requires centralized management across many services

## Risks

- Missing or misconfigured environment variables cause runtime failures that are difficult to diagnose without explicit validation
  Mitigation: Implement startup validation that checks for required configuration keys and fails fast with clear error messages identifying missing values
  Owner: engineering team
- Environment variables are logged or exposed in error messages, leaking credentials
  Mitigation: Implement logging and error handling policies that redact configuration values and only expose key names in diagnostic output
  Owner: engineering team
- Configuration drift between environments causes inconsistent behavior that is difficult to reproduce locally
  Mitigation: Maintain environment-specific configuration documentation and use infrastructure-as-code to manage environment variable deployment consistently
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Implement startup validation that reads all required configuration keys and fails immediately with descriptive errors if any are missing or empty, preventing partial initialization
- Group related configuration keys using a consistent prefix pattern that identifies the external service, making it clear which keys belong together and simplifying documentation
- Consider implementing a configuration module that encapsulates environment variable access and provides typed accessors, centralizing validation and error handling

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify configuration validation tests that verify required environment variables are checked at startup
- Locate integration test suites that exercise API functions with missing configuration to confirm appropriate error handling
- Identify static analysis or linting rules that detect direct credential usage and enforce environment-based configuration sourcing

Accept when:
- All internal API modules that integrate with external services source credentials and endpoints from environment variables without hardcoded values
- Startup validation confirms required configuration keys are present and fails fast with clear error messages identifying missing keys
- Webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application

## Enforcement

- Verified by: Code review checklist items that verify new external service integrations use environment-based configuration
- Verified by: Static analysis rules that detect hardcoded credentials or endpoint URLs in API modules
- Verified by: Integration tests that validate configuration error handling by running with missing environment variables
- Violation handling: Pull requests containing hardcoded credentials or endpoints are blocked until refactored to use environment configuration
- Violation handling: Static analysis failures for credential detection trigger build failures and require remediation before merge
- Violation handling: Production deployment checklists include verification that all required environment variables are configured in the target environment
- Exception process: Exceptions for non-sensitive static configuration may be granted through architecture review if the value truly never varies across environments
- Exception process: Temporary hardcoding during prototyping must be documented with tracking issues and removed before production deployment
- Exception process: All exceptions require documentation of the rationale and approval from the security team for any credential-related deviations