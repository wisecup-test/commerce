# Access Environment Variables Through Process Object for Configuration: Default Values Provided

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase uses runtime environment variables to configure external service integrations, including store domains, access tokens, and revalidation secrets
- Configuration values are accessed directly through the process object's environment property at multiple points in the application lifecycle
- Core libraries detected include next/cache, next/headers, next/og, next/navigation, clsx, path, and fs/promises, establishing a server-side rendering context
- The pattern spans utility modules, API integration layers, and component rendering logic, indicating system-wide configuration dependency
- Evidence shows coordination between environment-sourced secrets and runtime request handling through headers, cookies, and query parameters

## Problem Statement

Configuration values including credentials and secrets must be accessible throughout the application stack while maintaining separation between build-time constants and runtime secrets, without hardcoding sensitive values or exposing them to client-side execution contexts.

## Decision

1. MAY: Default values MAY be provided for non-sensitive configuration variables when environment values are absent

## Policy Block

- MAY Default values MAY be provided for non-sensitive configuration variables when environment values are absent

In scope:
- Server-side modules accessing external service credentials
- API integration layers requiring authentication tokens
- Utility modules providing configuration access patterns
- Component rendering logic requiring site metadata
- Request handlers validating webhook signatures or revalidation secrets

Out of scope:
- Client-side JavaScript bundles
- Static site generation contexts where runtime environment is unavailable
- Build-time configuration that must be embedded at compile time
- Public configuration values that can be safely exposed to browsers

Exceptions:
- EX-001: Public, non-sensitive configuration values may be exposed to client contexts when explicitly prefixed with a public indicator

## Rationale

- Evidence shows consistent use of process.env access across 4 files with 87.52% confidence, indicating an established pattern for configuration management
- The pattern separates secret storage from code, enabling different credentials per deployment environment without code changes
- Direct environment variable access integrates naturally with server-side rendering frameworks that provide process object access in server contexts
- The detection of validation logic for revalidation secrets demonstrates awareness of security requirements when handling environment-sourced credentials

## Consequences

Positive:
- Credentials remain external to source code, reducing risk of accidental exposure in version control
- Different deployment environments can use distinct configuration values without code modification
- Server-side execution contexts maintain access to sensitive credentials while preventing client-side exposure
- Standard environment variable patterns integrate with container orchestration and deployment platforms

Negative:
- Runtime configuration errors may not surface until specific code paths execute, delaying failure detection
- Direct process.env access scattered across modules creates implicit dependencies that are harder to test
- Missing or misconfigured environment variables can cause runtime failures in production if not validated at startup
- Type safety is lost when accessing environment variables as strings without schema validation

## Alternatives

- Centralize all environment variable access in a typed configuration module with validation and schema enforcement (rejected)
  Rejected because: Evidence shows direct process.env access distributed across multiple modules without centralized configuration abstraction
  When valid: Valid for new projects or during major refactoring when type safety and validation can be introduced systematically
- Use build-time environment variable substitution to embed configuration values at compile time (rejected)
  Rejected because: Pattern shows runtime access through process object, indicating need for deployment-time configuration flexibility
  When valid: Valid for truly static configuration that never varies between deployments of the same build artifact
- Load configuration from external configuration services or secret management systems (rejected)
  Rejected because: No evidence of external configuration service integration; pattern relies on environment variables as the configuration source
  When valid: Valid for complex multi-service architectures requiring centralized secret rotation and audit capabilities

## Risks

- Missing or misconfigured environment variables cause runtime failures that may not be detected until specific features are accessed
  Mitigation: Implement startup validation that checks for required environment variables and fails fast with clear error messages
  Owner: engineering team
- Accidental exposure of environment variables to client-side bundles through improper context usage
  Mitigation: Enforce server-side only access patterns and use build-time analysis to detect client-side environment variable references
  Owner: security team
- Lack of type safety when accessing environment variables as strings leads to runtime type errors
  Mitigation: Consider introducing schema validation or typed configuration wrappers for critical configuration values
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
- Identify all modules that access environment variables and document the expected variable names, types, and whether they are required or optional
- Implement validation logic that executes during application initialization to verify required environment variables are present and contain valid values
- Ensure environment variable access only occurs in server-side execution contexts by reviewing module boundaries and execution environment

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the testing framework; locate and execute the project's test suite to verify environment variable handling
- Discover the project's linting configuration and execute static analysis to detect any environment variable access in client-side contexts
- Discover the project's build configuration and execute a production build to verify no secrets are embedded in client bundles

Accept when:
- All tests pass including those that verify environment variable validation and error handling for missing configuration
- Static analysis confirms no environment variable access occurs in client-side execution contexts
- Production build verification shows no sensitive environment variables embedded in client-accessible bundles

## Enforcement

- Verified by: Automated testing that validates environment variable handling and missing configuration scenarios
- Verified by: Static analysis during continuous integration to detect client-side environment variable access
- Verified by: Code review checklist items for new environment variable introductions
- Verified by: Security scanning of build artifacts to detect embedded secrets
- Violation handling: Build failures when static analysis detects environment variable access in client contexts
- Violation handling: Test failures when required environment variables are not properly validated
- Violation handling: Code review rejection for direct environment variable access without validation
- Violation handling: Security incident response for any secrets found in client-accessible bundles
- Exception process: Document the specific environment variable and justification for exception
- Exception process: Obtain security team approval for any client-side environment variable exposure
- Exception process: Add explicit comments in code explaining the exception and security review outcome
- Exception process: Record exception in security documentation with periodic review schedule