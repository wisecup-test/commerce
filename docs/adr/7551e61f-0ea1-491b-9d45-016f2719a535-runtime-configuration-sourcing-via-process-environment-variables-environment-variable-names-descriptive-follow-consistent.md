# Runtime Configuration Sourcing via Process Environment Variables: Environment Variable Names Descriptive Follow Consistent

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The application requires dynamic configuration values that vary between deployment environments.
- Sensitive information, such as API tokens and domain names, needs to be managed securely outside of source code.
- Multiple modules and components across the codebase depend on these external configuration parameters.
- A consistent mechanism for accessing these runtime-specific values is necessary to ensure maintainability and prevent hardcoding.

## Decision

1. SHOULD: Environment variable names SHOULD be descriptive and follow a consistent naming convention (e.g., `UPPER_SNAKE_CASE`).

## Policy Block

- SHOULD Environment variable names SHOULD be descriptive and follow a consistent naming convention (e.g., `UPPER_SNAKE_CASE`).

In scope:
- Any module or component requiring runtime-specific configuration values.
- Any module or component handling sensitive credentials or API keys.

Out of scope:
- Static configuration values that are part of the build process.
- Configuration values that are managed by a dedicated configuration service or database.

## Rationale

- Using environment variables via `process.env` provides a standard and widely understood mechanism for externalizing configuration.
- This approach enhances security by keeping sensitive data out of version control.
- It promotes portability across different deployment environments (development, staging, production) without requiring code changes.
- The observed pattern demonstrates consistent adoption of this mechanism across various parts of the codebase.

## Consequences

Positive:
- Improved security posture by separating secrets from source code.
- Increased flexibility for deployment across different environments.
- Simplified configuration management for operations teams.
- Reduced risk of accidental exposure of sensitive data.

Negative:
- Requires careful management of environment variables in deployment pipelines.
- Potential for runtime errors if required environment variables are not set.
- Debugging can be more complex if environment variable values are not easily inspectable.

## Alternatives

- Hardcoding configuration values directly in source code. (rejected)
  Rejected because: This approach introduces security risks, reduces flexibility, and makes environment-specific deployments difficult.
  When valid: Only for truly static, non-sensitive values that never change.
- Using a dedicated configuration file (e.g., `.env` files with a library like `dotenv`). (deferred)
  Rejected because: While offering similar benefits, the current evidence shows direct `process.env` usage without an explicit configuration file library.
  When valid: For local development or when a more structured approach to loading environment variables from files is desired.

## Risks

- Missing environment variables at runtime leading to application failures.
  Mitigation: Implement robust validation and default values where appropriate; ensure deployment pipelines enforce required variables.
  Owner: engineering team
- Accidental exposure of sensitive environment variables through logs or error messages.
  Mitigation: Implement secure logging practices; avoid logging raw environment variable values.
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
- Developers should document all required environment variables and their purpose.
- Consider using schema validation for environment variables in critical paths to ensure correctness and presence.

## Continuation Context


Verify commands:
- Discover the project's build scripts and execute them to ensure successful compilation.
- Discover the project's test suite and run all tests to confirm application functionality.
- Inspect the deployed application's runtime environment to confirm environment variables are correctly set and accessed.

Accept when:
- The application successfully starts and operates without configuration-related errors.
- Sensitive information is not hardcoded in the codebase.
- Changes to environment variables correctly alter application behavior without code modification.

## Enforcement

- Verified by: Automated CI checks for `process.env` usage; code reviews.
- Violation handling: CI pipeline failure; code review rejection.
- Exception process: Formal architectural review and approval by a lead engineer or architect.