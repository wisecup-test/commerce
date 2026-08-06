# Access Environment Variables Through Process Object for Runtime Configuration: Sensitive Credentials Such

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase accesses runtime configuration values including store domains, access tokens, revalidation secrets, site names, and deployment URLs through a process object's environment property
- Public API contracts in lib/shopify/index.ts, components/opengraph-image.tsx, components/icons/logo.tsx, and lib/utils.ts retrieve configuration from the process environment at runtime
- The pattern appears across 4 files with 87.52% confidence, indicating consistent adoption of environment-based configuration for both sensitive credentials and non-sensitive settings
- The shopifyFetch function constructs external HTTP requests with headers containing access tokens retrieved from the process environment, establishing a boundary between internal configuration and external service communication

## Problem Statement

Applications require a secure, consistent mechanism to access runtime configuration values including sensitive credentials without hardcoding them in source code, while maintaining clear boundaries between configuration sources and application logic that consumes them.

## Decision

1. MUST: Sensitive credentials such as access tokens and revalidation secrets must never be hardcoded in source files or committed to version control

## Policy Block

- MUST Sensitive credentials such as access tokens and revalidation secrets must never be hardcoded in source files or committed to version control

In scope:
- All modules that construct HTTP requests to external services
- All components that render dynamic content based on deployment environment
- All utility functions that validate or transform configuration values
- All API route handlers that verify request authenticity using secrets

Out of scope:
- Static content that does not vary by environment
- Type definitions and interface declarations
- Pure functions that operate only on their parameters
- Client-side code that runs in browser environments without access to server-side environment variables

## Rationale

- The evidence shows consistent use of process.env across 4 files to access 7 distinct configuration values including SHOPIFY_STORE_DOMAIN, SHOPIFY_STOREFRONT_ACCESS_TOKEN, SHOPIFY_REVALIDATION_SECRET, SITE_NAME, and VERCEL_PROJECT_PRODUCTION_URL
- The shopifyFetch function demonstrates the pattern's application at service boundaries, where credentials are retrieved from the environment and injected into HTTP headers for external API authentication
- Accessing configuration through the process environment enables deployment-time configuration injection without modifying source code, supporting multiple deployment targets with different credentials
- The pattern establishes a clear separation between configuration sources and application logic, making it easier to audit credential usage and rotate secrets without code changes

## Consequences

Positive:
- Credentials and configuration can be changed at deployment time without modifying or redeploying source code
- Sensitive values are kept out of version control, reducing the risk of credential leakage through repository access
- The same codebase can be deployed to multiple environments with different configuration by changing environment variables
- Configuration access points are explicit and searchable, making it easier to audit which parts of the codebase use which credentials

Negative:
- Runtime errors may occur if required environment variables are not set, potentially causing application failures that are only discovered at runtime
- The lack of compile-time validation means typos in environment variable names will not be caught until the code executes
- Debugging configuration issues requires access to the deployment environment, which may not be available to all developers
- The pattern does not inherently provide type safety or validation for configuration values retrieved from the environment

## Alternatives

- Hardcode configuration values directly in source files (rejected)
  Rejected because: Hardcoding credentials in source code creates security vulnerabilities by committing secrets to version control and prevents using different configuration across deployment environments
  When valid: Never valid for sensitive credentials; only acceptable for truly static values that never vary by environment
- Use a configuration file that is loaded at runtime and excluded from version control (rejected)
  Rejected because: Configuration files require file system access and additional parsing logic, while environment variables are universally supported by deployment platforms and container orchestration systems
  When valid: Valid for complex configuration with nested structures that exceed the capabilities of flat environment variables
- Retrieve configuration from a remote configuration service at application startup (rejected)
  Rejected because: Remote configuration services introduce additional dependencies, network latency, and failure modes during application initialization, while the current pattern requires no external services
  When valid: Valid for applications requiring dynamic configuration updates without redeployment or centralized configuration management across many services

## Risks

- Missing or misconfigured environment variables cause runtime failures that are difficult to diagnose in production
  Mitigation: Implement validation functions that check for required environment variables at application startup and fail fast with clear error messages
  Owner: engineering team
- Environment variables may be logged or exposed through error messages, leaking sensitive credentials
  Mitigation: Configure logging and error handling to redact environment variable values and implement monitoring to detect credential exposure
  Owner: engineering team
- Lack of type safety means invalid configuration values may not be detected until runtime execution reaches the code path that uses them
  Mitigation: Create typed configuration objects that parse and validate environment variables at startup, providing type-safe access throughout the application
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
- Create a centralized validation module that checks for required environment variables at application startup and exports typed configuration objects, preventing runtime errors from missing configuration
- Document all required environment variables in a template or example file that developers can copy to set up their local development environment
- Consider implementing a configuration schema that validates not just the presence but also the format and constraints of environment variable values

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the runtime environment; locate any validation or startup scripts that check environment variable presence
- Search the codebase for all references to the process environment property and verify that no credentials are hardcoded in source files
- Identify the project's testing framework and locate test suites that verify configuration loading behavior and error handling for missing environment variables

Accept when:
- All configuration values including credentials are accessed through the process environment property and no hardcoded secrets exist in source files
- Validation logic exists that checks for required environment variables and provides clear error messages when they are missing
- Tests verify that the application handles missing or invalid environment variables appropriately

## Enforcement

- Verified by: Code review checks that new configuration values are accessed through the process environment
- Verified by: Static analysis tools scan for hardcoded credentials and flag violations
- Verified by: Automated tests verify that configuration loading and validation logic works correctly
- Violation handling: Pull requests containing hardcoded credentials are rejected and must be revised before merging
- Violation handling: Detected credential leaks trigger immediate credential rotation and security incident response
- Violation handling: Code that accesses configuration without validation is flagged for refactoring during code review
- Exception process: Exceptions for truly static values that never vary by environment may be approved by the security team with documentation of the rationale
- Exception process: Temporary hardcoding during local development is acceptable but must be removed before committing code
- Exception process: Alternative configuration mechanisms require architectural review and approval before adoption