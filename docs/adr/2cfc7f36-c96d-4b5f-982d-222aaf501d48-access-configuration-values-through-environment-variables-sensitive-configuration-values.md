# Access Configuration Values Through Environment Variables: Sensitive Configuration Values

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The application requires runtime configuration values including store domain identifiers, access tokens, revalidation secrets, and site naming metadata that vary across deployment environments
- Configuration values must be accessible to both server-side request handlers and build-time image generation functions without hardcoding environment-specific values in source code
- The runtime environment provides a process-level interface for accessing externally-injected configuration through environment variables
- Multiple modules across the codebase require access to the same configuration values, necessitating a consistent access pattern

## Problem Statement

The application needs a consistent mechanism to access deployment-specific configuration values such as API credentials, domain identifiers, and application metadata across different execution contexts without embedding sensitive or environment-specific values directly in source code.

## Decision

1. MUST: Sensitive configuration values including access tokens, secrets, and credentials SHALL be retrieved exclusively through environment variables

## Policy Block

- MUST Sensitive configuration values including access tokens, secrets, and credentials SHALL be retrieved exclusively through environment variables

In scope:
- All server-side request handling code requiring external service credentials
- Build-time functions requiring application metadata
- API client initialization requiring endpoint configuration
- Webhook validation requiring shared secrets
- Any module requiring deployment-environment-specific values

Out of scope:
- Static constant values that are identical across all environments
- Type definitions and interface declarations
- Client-side code executing in browser contexts
- Build-time constants derived from package metadata

## Rationale

- The IR evidence shows consistent use of process.env accessor pattern across multiple modules for SHOPIFY_STORE_DOMAIN, SHOPIFY_STOREFRONT_ACCESS_TOKEN, SHOPIFY_REVALIDATION_SECRET, and SITE_NAME, demonstrating an established architectural pattern
- Environment variable access enables the same codebase to operate across development, staging, and production environments without source code modifications
- Separating configuration from code reduces the risk of accidentally committing sensitive credentials to version control systems
- The pattern supports the security.secrets_handling facet by providing a standard mechanism for accessing sensitive values without embedding them in source files

## Consequences

Positive:
- Configuration values can be changed across deployments without modifying or rebuilding source code
- Sensitive credentials remain external to the codebase and version control system
- The same application binary can be deployed to multiple environments with different configuration
- Configuration management can be handled through deployment tooling and secret management systems

Negative:
- Runtime errors may occur if required environment variables are not set, potentially causing application startup failures
- Configuration schema and required variables are not enforced at compile time, requiring runtime validation
- Debugging configuration issues requires access to the deployment environment rather than examining source code alone
- Type safety for configuration values is limited without additional validation layers

## Alternatives

- Embed configuration values directly in source code files with separate builds per environment (rejected)
  Rejected because: Requires separate build artifacts per environment, increases risk of credential exposure in version control, and violates separation of code and configuration principles
  When valid: Only appropriate for truly static values that never vary across environments and contain no sensitive data
- Use configuration files loaded from the filesystem at runtime (rejected)
  Rejected because: Adds filesystem dependency, complicates deployment by requiring file distribution alongside binaries, and provides no security advantage over environment variables for secret handling
  When valid: May be appropriate for complex structured configuration that exceeds the practical limits of environment variable formats
- Fetch configuration from a remote configuration service at application startup (rejected)
  Rejected because: Introduces external service dependency for application bootstrap, adds network latency to startup, and increases operational complexity
  When valid: Appropriate for dynamic configuration that changes during application runtime or for centralized configuration management across large service fleets

## Risks

- Missing or misconfigured environment variables cause runtime failures that may not be detected until deployment
  Mitigation: Implement startup validation that checks for required environment variables and fails fast with clear error messages. Document all required variables in deployment guides.
  Owner: engineering team
- Environment variables may be logged or exposed through error messages, leaking sensitive credentials
  Mitigation: Implement logging filters that redact environment variable values. Ensure error handling does not echo configuration values in user-facing messages.
  Owner: engineering team
- Type coercion and undefined handling may cause subtle bugs when environment variables are missing or malformed
  Mitigation: Add explicit type validation and parsing for configuration values at access points. Consider a typed configuration layer that validates and transforms raw environment variables.
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
- Create a centralized configuration module that accesses all environment variables in one location, providing typed accessors and validation. This reduces duplication and enables consistent error handling.
- Document all required environment variables in deployment documentation, including their purpose, format requirements, and whether they contain sensitive data requiring secret management.
- Consider implementing a startup validation function that verifies all required environment variables are present and properly formatted before the application begins processing requests, enabling fail-fast behavior with clear diagnostics.

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the runtime environment. Locate any startup validation or configuration loading module and execute it to verify required environment variables are documented.
- Search the codebase for all environment variable access patterns. Verify that sensitive values are accessed through the process environment interface and not hardcoded.
- Locate the project's testing configuration. Discover and execute any integration tests that validate configuration loading behavior with missing or malformed environment variables.

Accept when:
- All configuration access uses the process environment interface pattern without hardcoded environment-specific values in source files
- Sensitive credentials including tokens and secrets are accessed exclusively through environment variables
- The application provides clear error messages when required environment variables are missing or invalid

## Enforcement

- Verified by: Code review verification that no sensitive values are hardcoded in source files
- Verified by: Static analysis scanning for hardcoded credentials and configuration values
- Verified by: Integration test validation that application startup fails appropriately when required environment variables are missing
- Violation handling: Pull requests containing hardcoded credentials or environment-specific values are rejected
- Violation handling: Static analysis failures block deployment pipeline progression
- Violation handling: Discovered violations in production code trigger immediate remediation tickets
- Exception process: Exceptions for truly static, non-sensitive values may be granted through architecture review
- Exception process: Exception requests must document why environment variable access is inappropriate for the specific use case
- Exception process: Approved exceptions are documented in code comments with reference to the approval decision