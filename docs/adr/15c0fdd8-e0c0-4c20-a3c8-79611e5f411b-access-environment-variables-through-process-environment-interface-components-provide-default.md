# Access Environment Variables Through Process Environment Interface: Components Provide Default

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase requires runtime configuration values for external service integration, including store domains, access tokens, revalidation secrets, site names, and deployment URLs
- Configuration values must be accessible across multiple modules including API clients, UI components, utility functions, and webhook handlers
- The runtime environment provides a process interface that exposes environment variables as key-value pairs
- Direct access to environment variables through the process interface is observed in 4 files with consistent patterns for retrieving store domain, storefront access token, revalidation secret, site name, and production URL values

## Problem Statement

The application needs a consistent mechanism to retrieve runtime configuration values from the execution environment without hardcoding sensitive credentials or environment-specific settings into source code, while ensuring these values are accessible across different architectural layers including API clients, UI rendering components, and utility modules.

## Decision

1. MAY: Components MAY provide default fallback values for non-sensitive configuration when environment variables are undefined

## Policy Block

- MAY Components MAY provide default fallback values for non-sensitive configuration when environment variables are undefined

In scope:
- API client modules that connect to external services
- UI components that render environment-specific content
- Utility functions that construct URLs or validate requests
- Webhook handlers that verify request authenticity
- Configuration validation logic

Out of scope:
- Build-time constants that are known at compile time
- Type definitions and interface declarations
- Static asset paths and resource identifiers
- Test fixtures and mock data

## Rationale

- The evidence shows consistent use of process environment access across 4 files for retrieving store domain, storefront access token, revalidation secret, site name, and production URL values
- This pattern separates configuration from code, enabling the same codebase to operate in different environments without source modifications
- Environment variable access through the process interface is the standard mechanism in the detected runtime for externalizing configuration
- The pattern supports security best practices by preventing credential exposure in version control while maintaining accessibility at runtime

## Consequences

Positive:
- Configuration values can be changed without modifying or redeploying source code
- Sensitive credentials remain external to the codebase and version control system
- The same application code can operate across development, staging, and production environments
- Environment-specific settings are isolated from business logic and presentation layers

Negative:
- Runtime failures occur if required environment variables are missing or misconfigured
- Debugging configuration issues requires access to the deployment environment rather than source code inspection
- Type safety for environment variable values is not enforced by the language runtime
- Environment variable naming collisions may occur across different modules or services

## Alternatives

- Hardcode configuration values directly in source files (rejected)
  Rejected because: Exposes sensitive credentials in version control, prevents environment-specific deployments, and violates security best practices
  When valid: Never valid for production systems with sensitive credentials or multi-environment deployments
- Use configuration files loaded from the filesystem at runtime (rejected)
  Rejected because: Requires file system access and management, complicates deployment pipelines, and does not align with observed evidence showing process environment usage
  When valid: Valid for complex configuration hierarchies with nested structures that exceed environment variable capabilities
- Retrieve configuration from remote configuration service at runtime (rejected)
  Rejected because: Introduces external dependency and network latency for configuration access, adds complexity not evidenced in the codebase
  When valid: Valid for distributed systems requiring dynamic configuration updates without redeployment

## Risks

- Missing or misconfigured environment variables cause runtime failures that may not be detected until deployment
  Mitigation: Implement validation logic that checks required environment variables at application startup and fails fast with clear error messages
  Owner: engineering team
- Environment variable values lack type safety and may contain invalid formats or values
  Mitigation: Create validation utilities that parse and verify environment variable formats, types, and constraints before use
  Owner: engineering team
- Sensitive credentials in environment variables may be exposed through logging, error messages, or debugging output
  Mitigation: Implement logging filters and error handlers that redact sensitive environment variable values from all output
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
- Create a centralized configuration module that accesses all required environment variables and exports typed configuration objects with validation
- Document all required and optional environment variables in the repository with descriptions, expected formats, and example values
- Use environment variable validation utilities to check for required values at application startup and provide clear error messages for missing or invalid configuration

## Continuation Context


Verify commands:
- Discover the project's environment variable validation script in the repository and execute it to verify all required configuration values are defined
- Locate the project's test suite and run integration tests that verify environment variable access patterns across all modules
- Identify the project's static analysis or linting configuration and execute checks that detect hardcoded credentials or configuration values in source files

Accept when:
- All required environment variables are successfully retrieved through the process environment interface without runtime errors
- No hardcoded credentials, tokens, or sensitive configuration values are present in source files
- Environment variable validation logic correctly identifies missing or invalid configuration at startup

## Enforcement

- Verified by: Static analysis tools scan source files for hardcoded credentials and configuration values
- Verified by: Code review process verifies that new configuration values use environment variables
- Verified by: Integration tests validate environment variable access patterns in deployment environments
- Violation handling: Build pipeline fails if static analysis detects hardcoded credentials in source files
- Violation handling: Code review blocks merge requests that introduce hardcoded configuration values
- Violation handling: Runtime validation fails fast at startup if required environment variables are missing
- Exception process: Non-sensitive default values may be hardcoded with explicit documentation and code review approval
- Exception process: Test fixtures and mock data may contain hardcoded values with clear separation from production code paths
- Exception process: Build-time constants known at compile time may be embedded in source with architectural review