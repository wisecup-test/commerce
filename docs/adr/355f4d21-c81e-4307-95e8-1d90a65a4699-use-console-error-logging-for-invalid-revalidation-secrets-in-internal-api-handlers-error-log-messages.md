# Use Console Error Logging for Invalid Revalidation Secrets in Internal API Handlers: Error Log Messages

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Internal API handlers coordinate with external service webhooks and require secret validation to prevent unauthorized revalidation requests
- The codebase uses runtime environment variables to configure store domain, storefront access tokens, and revalidation secrets for external service integration
- Request handling extracts cache identifiers from cookies, topic headers from incoming requests, and secret parameters from query strings to validate webhook authenticity
- Console-based error logging provides immediate visibility into security validation failures during request processing without requiring structured logging infrastructure
- The pattern separates concerns between external client communication via fetch with custom headers and internal service boundary validation using cookies and headers

## Problem Statement

Internal API handlers that validate webhook secrets need a lightweight, synchronous mechanism to record security validation failures without introducing dependencies on structured logging frameworks or observability platforms, while maintaining visibility into authentication errors during development and production debugging.

## Decision

1. SHOULD: Error log messages SHOULD clearly identify the validation failure reason without exposing secret values

## Policy Block

- SHOULD Error log messages SHOULD clearly identify the validation failure reason without exposing secret values

In scope:
- Internal API route handlers that validate webhook secrets
- Request processing functions that extract secrets from query parameters
- Security validation logic that compares request secrets against environment configuration
- Error logging for authentication and authorization failures in internal APIs

Out of scope:
- Public API endpoints that do not require secret validation
- Client-side logging or browser console output
- Structured logging to external observability platforms
- Application-level business logic errors unrelated to security validation

## Rationale

- Console error logging provides zero-dependency, synchronous error recording that works in both development and production environments without requiring observability infrastructure setup
- The pattern aligns with the existing architecture where internal APIs coordinate external service webhooks and require immediate visibility into security validation failures
- Using console output for security validation errors maintains separation between lightweight error recording and optional structured logging, allowing teams to add observability incrementally
- The evidence shows a clear pattern of extracting secrets from environment variables and validating them against request parameters, requiring a simple logging mechanism for failure cases

## Consequences

Positive:
- Immediate visibility into security validation failures without requiring structured logging infrastructure or third-party dependencies
- Synchronous error recording ensures validation failures are captured even if asynchronous logging systems fail or are unavailable
- Simple implementation reduces cognitive overhead for developers implementing webhook validation logic
- Console output integrates naturally with container logs and standard output capture in production environments

Negative:
- Console logging lacks structured metadata, making automated alerting and aggregation more difficult compared to structured logging solutions
- Error messages in console output may be harder to filter and search in high-volume production environments
- No built-in support for log levels, sampling, or rate limiting that structured logging frameworks provide
- Teams must implement additional tooling to convert console output into actionable security alerts

## Alternatives

- Use a structured logging framework with JSON output and log levels (rejected)
  Rejected because: Introduces external dependencies and configuration overhead for a simple security validation use case where immediate console visibility is sufficient
  When valid: When the application already uses a structured logging framework or requires automated alerting and log aggregation
- Silent failure with no logging for invalid secrets (rejected)
  Rejected because: Eliminates visibility into security validation failures, making debugging and security monitoring impossible
  When valid: Never valid for security-related validation failures
- Throw exceptions for invalid secrets without logging (rejected)
  Rejected because: Exception stack traces may expose sensitive information and do not provide controlled error messaging for security events
  When valid: When the framework provides exception handlers that log securely and the application requires request termination on validation failure

## Risks

- Console error messages may inadvertently expose secret values or sensitive request details if not carefully formatted
  Mitigation: Review all console error statements to ensure they describe the failure without including actual secret values, tokens, or sensitive parameters
  Owner: engineering team
- High-volume invalid secret attempts may flood console output and obscure other critical errors
  Mitigation: Implement rate limiting or request throttling at the API gateway level to prevent console log flooding from repeated validation failures
  Owner: engineering team
- Console logging provides no built-in alerting mechanism for security validation failures
  Mitigation: Configure log aggregation tools to monitor console output for security-related error patterns and trigger alerts when validation failures exceed thresholds
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
- Locate internal API handlers that validate webhook secrets by searching for functions that extract secret parameters from request query strings and compare them against environment configuration
- Ensure error messages clearly indicate validation failure without including the expected or received secret values in the console output
- Consider wrapping console error calls in a utility function that enforces consistent error message formatting and prevents accidental secret exposure

## Continuation Context


Verify commands:
- Discover the project's test suite location and execute tests that validate internal API handlers reject requests with invalid or missing revalidation secrets
- Locate and run the project's security validation test cases to confirm console error output appears for authentication failures
- Identify the project's integration test configuration and verify webhook handler behavior with invalid secrets produces expected console error messages

Accept when:
- Internal API handlers reject requests with invalid revalidation secrets and produce console error output describing the validation failure
- Console error messages do not expose actual secret values, tokens, or sensitive configuration in their output
- Test coverage confirms that all secret validation failure paths produce appropriate console error logging

## Enforcement

- Verified by: Code review verification that console error statements do not expose secret values
- Verified by: Automated test suite execution confirming error logging behavior for invalid secrets
- Verified by: Security audit of internal API handlers to verify validation failure visibility
- Violation handling: Pull requests that add secret validation without console error logging must be revised before merge
- Violation handling: Code that exposes secret values in console output must be immediately patched
- Violation handling: Missing test coverage for validation failure logging triggers CI failure
- Exception process: Teams may request exceptions to use structured logging instead of console output if the application already has a logging framework integrated
- Exception process: Exceptions require documentation of the alternative logging mechanism and confirmation that security validation failures remain visible
- Exception process: Security team must approve any exception that affects visibility of authentication failures