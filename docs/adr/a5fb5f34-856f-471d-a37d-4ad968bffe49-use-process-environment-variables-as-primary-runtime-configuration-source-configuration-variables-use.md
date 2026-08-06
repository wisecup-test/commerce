# Use Process Environment Variables as Primary Runtime Configuration Source: Configuration Variables Use

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase requires runtime configuration values for external service integration, including store domain endpoints, access tokens, and revalidation secrets
- Configuration values must be accessible across multiple runtime contexts including server-side rendering, API routes, and image generation functions
- The application integrates with external services requiring authentication credentials and endpoint configuration that varies between deployment environments
- Runtime configuration needs to support both application-level settings and security-sensitive credentials through a unified access pattern

## Problem Statement

The application requires a consistent, environment-agnostic mechanism to inject runtime configuration and credentials that works across server-side contexts, supports deployment environment variation, and maintains separation between code and configuration without hardcoding values or requiring build-time substitution.

## Decision

1. SHOULD: Configuration variables SHOULD use descriptive prefixes that identify the service or domain they configure

## Policy Block

- SHOULD Configuration variables SHOULD use descriptive prefixes that identify the service or domain they configure

In scope:
- All server-side runtime contexts including API routes, server components, and middleware
- Configuration for external service integration including authentication credentials and endpoint URLs
- Application-level settings that vary between deployment environments
- Image generation and dynamic content rendering functions requiring environment-specific configuration

Out of scope:
- Client-side JavaScript code where process environment variables are not accessible
- Build-time configuration that must be resolved during compilation
- Static configuration values that are identical across all environments
- Type definitions and interface declarations

## Rationale

- Process environment variables provide a standard, platform-agnostic mechanism for configuration injection supported across deployment targets including containers, serverless functions, and traditional servers
- The evidence shows consistent usage of process.env accessor across 2 files with 88.15% confidence, accessing variables for store domain, access tokens, revalidation secrets, and site naming
- Environment variable configuration enables the twelve-factor app principle of strict separation between code and config, allowing the same codebase to be deployed across multiple environments without modification
- The pattern supports both security requirements (credentials never in source control) and operational requirements (environment-specific configuration without rebuilds)

## Consequences

Positive:
- Configuration can be changed between deployments without code changes or rebuilds, enabling rapid environment-specific adjustments
- Security-sensitive credentials remain outside source control and can be managed through secure secret management systems
- The same application artifact can be promoted across environments with only configuration changes
- Platform portability is maintained as environment variables are universally supported across hosting platforms

Negative:
- Configuration errors may only be detected at runtime when variables are accessed, rather than at build time
- Missing or misconfigured environment variables can cause runtime failures that are harder to diagnose than compile-time errors
- No type safety or validation is provided by default for environment variable values
- Configuration documentation must be maintained separately from code, increasing the risk of drift between expected and actual configuration

## Alternatives

- Use build-time configuration substitution to inject values during compilation (rejected)
  Rejected because: Build-time substitution requires rebuilding the application for each environment, violating the principle of building once and deploying many times, and prevents runtime configuration updates
  When valid: When configuration values are truly static across all environments and will never change without a code deployment
- Use configuration files with environment-specific overrides loaded at application startup (rejected)
  Rejected because: Configuration files must be managed alongside code or mounted separately, complicating deployment and increasing the risk of committing secrets to source control
  When valid: When configuration is complex with nested structures that are difficult to express as flat environment variables
- Use a centralized configuration service with runtime fetching (rejected)
  Rejected because: Adds external dependency and latency for configuration access, increases system complexity, and introduces a potential single point of failure
  When valid: When configuration must be dynamically updated across multiple running instances without restart or when configuration requires audit trails and versioning

## Risks

- Missing or misconfigured environment variables may cause runtime failures that are difficult to diagnose, especially in production environments where debugging access is limited
  Mitigation: Implement startup validation that checks for required environment variables and fails fast with clear error messages. Document all required variables with examples and validation rules.
  Owner: engineering team
- Environment variables are strings without type information, leading to potential type coercion errors when numeric or boolean values are expected
  Mitigation: Create typed configuration accessor functions that parse and validate environment variable values with appropriate error handling and type conversion
  Owner: engineering team
- Secrets stored as environment variables may be exposed through error messages, logging, or process inspection if not handled carefully
  Mitigation: Implement logging filters to redact sensitive values, avoid including configuration in error messages, and use platform-specific secret management features where available
  Owner: security team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Create a centralized configuration module that accesses all environment variables through typed accessor functions with validation, providing a single source of truth for configuration schema
- Document all required and optional environment variables in a dedicated configuration reference, including expected formats, example values, and validation rules
- Implement startup validation that checks for required environment variables before the application begins serving requests, failing fast with actionable error messages that identify missing or invalid configuration

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the runtime environment. Locate any startup validation or configuration loading scripts in the repository and execute them to verify required environment variables are documented
- Search the codebase for direct process environment accessor usage and verify all accesses follow the naming convention and are documented in the configuration reference
- Identify the project's testing framework from the dependency manifest and locate test files that verify configuration loading behavior with missing or invalid environment variables

Accept when:
- All runtime configuration values are sourced from process environment variables with no hardcoded credentials or environment-specific values in source code
- Configuration variable names use consistent uppercase-with-underscores naming convention and include descriptive prefixes
- Startup validation exists that checks for required environment variables and provides clear error messages for missing or invalid configuration

## Enforcement

- Verified by: Code review verification that no credentials or environment-specific values are hardcoded in source files
- Verified by: Static analysis scanning for hardcoded secrets and configuration values
- Verified by: Integration tests that verify application behavior with different environment variable configurations
- Violation handling: Pull requests containing hardcoded credentials or environment-specific configuration values must be rejected
- Violation handling: Static analysis findings of hardcoded secrets trigger immediate remediation and credential rotation
- Violation handling: Configuration accessed outside the approved pattern requires refactoring before merge
- Exception process: Exceptions for non-sensitive default values may be approved by the technical lead with documentation of the rationale
- Exception process: Build-time configuration substitution may be approved for truly static values that never vary between environments
- Exception process: All exceptions must be documented in the configuration reference with clear justification