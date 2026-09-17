# Runtime Configuration Management via `process.env`: Use Process Env Access Runtime Configuration

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- Application requires dynamic configuration values at runtime.
- Sensitive information like API tokens and domain names must be kept out of source control.
- The application integrates with external services (e.g., Shopify) and deploys to specific environments (e.g., Vercel).
- The Node.js runtime provides `process.env` for accessing environment variables.

## Problem Statement

Managing application configuration, especially sensitive credentials and environment-specific settings, in a secure, flexible, and maintainable way across different deployment environments.

## Decision

1. MUST: MUST use `process.env` to access runtime configuration values.

## Policy Block

- MUST MUST use `process.env` to access runtime configuration values.

In scope:
- Application modules requiring external service credentials.
- Application modules requiring environment-specific URLs or domain names.
- Any code that needs to adapt its behavior based on the deployment environment.

Out of scope:
- Static configuration values that are part of the application's build process.
- User-specific preferences stored in databases or user profiles.

## Rationale

- Using `process.env` provides a standard and secure mechanism for injecting configuration at runtime, adhering to 12-factor app principles.
- It separates configuration from code, allowing for environment-specific deployments without code changes.
- Prevents sensitive data from being committed to version control.
- The observed usage in `lib/utils.ts` and `lib/shopify/index.ts` demonstrates existing adoption for critical configuration.

## Consequences

Positive:
- Improved security by keeping sensitive data out of source control.
- Increased deployment flexibility across different environments (development, staging, production).
- Clear separation of concerns between code and configuration.
- Reduced risk of accidental exposure of credentials.

Negative:
- Requires careful management of environment variables in deployment pipelines.
- Potential for runtime errors if required environment variables are not set.
- Debugging can be more complex if environment variable values are not easily inspectable.

## Alternatives

- Configuration files (e.g., .env files, JSON, YAML) (rejected)
  Rejected because: `process.env` is already observed as the primary mechanism, and configuration files often require additional parsing logic or build steps, which is not evidenced.
  When valid: For local development where `.env` files are commonly used to populate `process.env`.
- Dedicated configuration service (e.g., AWS Secrets Manager, HashiCorp Vault) (rejected)
  Rejected because: The evidence does not suggest the complexity or overhead of a dedicated service is currently adopted or necessary for the observed pattern.
  When valid: For highly sensitive, frequently rotated secrets in large-scale, enterprise environments.

## Risks

- Missing environment variables at runtime.
  Mitigation: Implement runtime checks for required environment variables and provide clear error messages.
  Owner: engineering team
- Accidental exposure of environment variables in logs or error messages.
  Mitigation: Ensure logging practices redact sensitive information and avoid printing raw `process.env` contents.
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
- Consider using a validation library or custom utility to ensure all critical environment variables are present and correctly formatted at application startup.
- Document all required environment variables and their purpose in the project's README or deployment guide.

## Continuation Context


Verify commands:
- Discover the project's build scripts and execute them to ensure successful compilation.
- Discover the project's test scripts and execute them to ensure all tests pass.
- Discover the project's deployment scripts and simulate a deployment to verify environment variable injection.

Accept when:
- Application builds successfully without errors related to configuration.
- All automated tests pass, including those that rely on configured values.
- The application starts and operates correctly in a deployed environment, accessing configured values as expected.

## Enforcement

- Verified by: CI/CD pipeline checks
- Verified by: code reviews
- Verified by: runtime monitoring
- Violation handling: Automated build failures
- Violation handling: code review comments requiring remediation
- Violation handling: runtime alerts
- Exception process: Documented exception request process requiring approval from architectural review board or lead engineers