# Environment Variable Management using `process.env`: Not Hardcode Sensitive Configuration Values Directly

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- Applications require dynamic configuration values that vary across deployment environments.
- Sensitive information, such as API tokens and domain names, must not be hardcoded into the codebase.
- The application needs a mechanism to access environment-specific settings at runtime.
- The `process` global object provides access to the current Node.js process, including its environment variables.

## Problem Statement

The application needs a consistent and secure method to retrieve configuration values that are external to the codebase and specific to the runtime environment, without exposing sensitive data in source control.

## Decision

1. MUST_NOT: MUST NOT hardcode sensitive configuration values directly into source code.

## Policy Block

- MUST_NOT MUST NOT hardcode sensitive configuration values directly into source code.

In scope:
- Any code requiring environment-specific configuration.
- Modules handling API keys, domain names, or other deployment-dependent settings.

Out of scope:
- Configuration values that are static and part of the application's core logic.
- Configuration managed by dedicated configuration services or files (if such alternatives were adopted).

## Rationale

- Directly using `process.env` provides a straightforward and widely understood mechanism for accessing environment variables in Node.js environments.
- It decouples configuration from code, allowing for flexible deployments across different environments (e.g., development, staging, production).
- This approach helps prevent sensitive information from being committed to version control.

## Consequences

Positive:
- Improved security by keeping sensitive data out of source code.
- Increased deployment flexibility as configuration can be changed without code modifications.
- Standardized access to environment-specific settings.

Negative:
- Potential for runtime errors if required environment variables are not set.
- Requires careful management of environment variables in deployment pipelines.
- Lack of type safety for configuration values without additional validation layers.

## Alternatives

- Dedicated configuration files (e.g., .env files with a library like dotenv, or JSON/YAML config files). (rejected)
  Rejected because: The current evidence shows direct `process.env` usage without an explicit configuration file parsing library, indicating a preference for direct environment variable injection.
  When valid: For projects requiring more complex configuration structures, type validation, or local development without relying on shell environment variables.
- External configuration services (e.g., AWS Secrets Manager, HashiCorp Vault). (rejected)
  Rejected because: The current project does not exhibit integration with such services.
  When valid: For highly sensitive secrets management, centralized configuration across microservices, or advanced security requirements.

## Risks

- Missing environment variables at runtime.
  Mitigation: Implement runtime checks and provide clear error messages or default values.
  Owner: Engineering team.
- Accidental exposure of sensitive environment variables.
  Mitigation: Ensure proper access controls in deployment environments and follow least privilege principles.
  Owner: Engineering team.

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure all required environment variables are documented and their purpose is clear.
- Consider adding a validation layer to ensure environment variables are present and correctly formatted at application startup.

## Continuation Context


Verify commands:
- Discover the project's build or runtime environment setup scripts.
- Execute the project's environment variable loading or validation scripts.
- Inspect the running application's configuration values.

Accept when:
- The application successfully starts and accesses all necessary configuration without errors.
- Sensitive values are correctly loaded from the environment and not exposed.
- Configuration values reflect the intended environment-specific settings.

## Enforcement

- Verified by: Automated CI/CD pipeline checks for required environment variables.
- Verified by: Code reviews to ensure `process.env` is used appropriately and sensitive data is not hardcoded.
- Violation handling: Build failures in CI/CD if critical environment variables are missing.
- Violation handling: Code review comments requiring remediation for hardcoded sensitive values.
- Exception process: Exceptions require explicit approval from a lead engineer or architect, documented in a separate ADR or project documentation, detailing the specific justification and alternative mitigation strategies.