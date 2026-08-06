# Use Environment Variables for Runtime Configuration in Logging Context: External Service Domain

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system integrates with external service APIs requiring domain endpoints, access tokens, and webhook validation secrets that vary across deployment environments
- Configuration values are consumed at runtime by logging, cache layer operations, and external client boundary code that validates incoming requests and constructs outbound API calls
- The codebase uses server-side runtime functions to access cookies, headers, and query parameters alongside environment-sourced configuration, creating a unified configuration access pattern
- Error logging for invalid secrets demonstrates that configuration validation occurs at request time rather than application startup

## Problem Statement

Runtime configuration for logging, external API clients, and request validation must be sourced consistently across deployment environments without hardcoding sensitive credentials or environment-specific endpoints into application code, while maintaining visibility into configuration errors through logging mechanisms.

## Decision

1. MUST: External service domain endpoints, access tokens, and validation secrets must be read from distinct environment variables with descriptive names indicating their purpose

## Policy Block

- MUST External service domain endpoints, access tokens, and validation secrets must be read from distinct environment variables with descriptive names indicating their purpose

In scope:
- External API client configuration including domain endpoints and access tokens
- Webhook validation secrets used in request handlers
- Configuration values logged during error conditions
- Runtime configuration accessed by cache layer and boundary service code

Out of scope:
- Build-time configuration or compilation constants
- Client-side configuration exposed to browsers
- Static configuration embedded in markup or stylesheets
- Development-only configuration not deployed to production

## Rationale

- The evidence shows three distinct environment variables being accessed for store domain, access token, and revalidation secret, demonstrating a pattern of externalizing environment-specific configuration
- Console error logging for invalid secrets indicates that configuration errors must be observable at runtime, requiring logging integration with configuration validation
- The pattern enables deployment across multiple environments without code changes while maintaining security by keeping credentials out of source control
- Consistent configuration sourcing across logging, cache operations, and external clients reduces cognitive overhead and simplifies configuration management

## Consequences

Positive:
- Configuration can be changed across environments without modifying or redeploying application code
- Sensitive credentials remain external to source control and build artifacts
- Configuration errors are visible through logging output, enabling faster diagnosis of deployment issues
- The pattern scales to additional configuration values without architectural changes

Negative:
- Configuration errors may only surface at runtime rather than at build time or application startup
- Environment variable management becomes a deployment concern requiring coordination across infrastructure and application teams
- Missing or misconfigured environment variables can cause partial failures in specific request paths rather than failing fast at startup
- Logging configuration errors to console may expose sensitive information if logs are not properly secured

## Alternatives

- Load configuration from external configuration files deployed alongside the application (rejected)
  Rejected because: File-based configuration requires file system access and deployment coordination, while environment variables are universally supported across container and serverless deployment models
  When valid: When configuration is complex enough to require structured formats or when configuration must be shared across multiple processes without environment variable propagation
- Fetch configuration from a remote configuration service at application startup (rejected)
  Rejected because: Remote configuration introduces network dependencies and startup latency, while the evidence shows simple key-value configuration that does not require dynamic updates
  When valid: When configuration must be updated without redeployment or when configuration is shared across many service instances requiring centralized management
- Validate all required environment variables at application startup and fail fast if missing (deferred)
  Rejected because: Not rejected but not evidenced; the current pattern validates at request time, which may be intentional for serverless environments with lazy initialization
  When valid: When the application has a clear startup phase and all configuration is required for any request path, enabling fail-fast behavior

## Risks

- Missing or misconfigured environment variables may cause runtime failures in production that are difficult to diagnose without comprehensive logging
  Mitigation: Implement startup validation for critical configuration values and ensure all configuration access includes error logging with sufficient context
  Owner: engineering team
- Console error logging of configuration validation failures may inadvertently expose sensitive configuration values or system internals in log aggregation systems
  Mitigation: Review all configuration error logging to ensure sensitive values are redacted and implement log filtering or masking in log aggregation infrastructure
  Owner: engineering team
- Environment variable naming collisions across different subsystems or services may cause configuration to be applied incorrectly
  Mitigation: Establish and enforce a hierarchical naming convention with service or subsystem prefixes and document all environment variables in a central registry
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
- Create a centralized configuration module that encapsulates environment variable access and provides typed accessors with validation, ensuring consistent error handling and logging across all configuration consumers
- Document all required environment variables including their purpose, expected format, and whether they are required or optional, maintaining this documentation alongside deployment configuration
- Consider implementing configuration validation at application startup for critical values while maintaining request-time validation for webhook-specific secrets that may only be used conditionally

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify all environment variable references in the codebase to confirm they follow the hierarchical naming convention
- Locate the project's test suite and execute tests that validate configuration loading and error logging behavior
- Identify the project's deployment configuration and verify that all required environment variables are documented and provided in deployment templates

Accept when:
- All environment-specific configuration values are sourced from environment variables with no hardcoded credentials or endpoints in source code
- Configuration validation failures produce console error output with sufficient context for diagnosis without exposing sensitive values
- All required environment variables are documented and deployment configurations provide them consistently across environments

## Enforcement

- Verified by: Code review verification that no hardcoded credentials or environment-specific endpoints exist in source control
- Verified by: Automated static analysis scanning for hardcoded secrets or configuration values
- Verified by: Deployment validation checks confirming all required environment variables are present before deployment proceeds
- Violation handling: Pull requests containing hardcoded credentials or environment-specific configuration are rejected until externalized
- Violation handling: Static analysis findings for hardcoded secrets trigger immediate remediation and credential rotation
- Violation handling: Deployment failures due to missing environment variables block promotion to production until configuration is corrected
- Exception process: Exceptions for non-sensitive default values may be approved by the engineering lead with documentation of the rationale
- Exception process: Development-only configuration may use hardcoded defaults if clearly marked and excluded from production builds
- Exception process: All exceptions must be documented in the configuration registry and reviewed quarterly for continued validity