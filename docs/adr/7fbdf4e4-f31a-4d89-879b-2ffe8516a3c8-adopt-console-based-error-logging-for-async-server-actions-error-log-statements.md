# Adopt Console-Based Error Logging for Async Server Actions: Error Log Statements

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Server-side async functions handling cart operations and external API integration require error visibility during development and production debugging
- The codebase coordinates cache layer operations, cookie management, and external service calls that may fail at runtime
- Error handling patterns must support async/await control flow in server actions without blocking request processing
- Observability requirements exist for tracking failures in cart mutations, checkout redirects, and webhook validation

## Problem Statement

Server actions performing cart operations, external API calls, and webhook processing need a consistent mechanism to surface runtime errors for debugging and monitoring without introducing structured logging infrastructure or third-party observability dependencies.

## Decision

1. SHOULD: Error log statements SHOULD preserve the original exception object to maintain stack traces and error metadata

## Policy Block

- SHOULD Error log statements SHOULD preserve the original exception object to maintain stack traces and error metadata

In scope:
- Server-side async functions exported as public API contracts
- Cart mutation operations including add, remove, and update actions
- External API client functions performing network requests
- Webhook handlers processing external service callbacks
- Functions that coordinate cache layer operations and cookie management

Out of scope:
- Client-side error handling in browser contexts
- Synchronous utility functions without async operations
- Error boundaries in component rendering
- Build-time or compile-time error reporting

## Rationale

- Evidence shows console.error usage in async server actions handling cart operations and external API integration across 2 files with 89.55% confidence
- The pattern coordinates error visibility with async/await control flow in functions that manage cache layers, cookies, and external service boundaries
- Console-based logging provides immediate error visibility without requiring structured logging infrastructure or external observability services
- The approach aligns with the detected paradigm.concurrency_model showing async function exports as public API contracts

## Consequences

Positive:
- Immediate error visibility in development and production logs without additional infrastructure
- Minimal performance overhead for error logging in exception paths
- Preserved stack traces and error metadata for debugging async operation failures
- Consistent error handling pattern across cart operations, API clients, and webhook handlers

Negative:
- Console output lacks structured metadata for automated log aggregation and alerting
- No built-in error correlation across distributed async operations
- Limited filtering and search capabilities compared to structured logging solutions
- Production error logs may expose sensitive information if exception objects contain secrets

## Alternatives

- Adopt structured logging library with JSON output and log levels (rejected)
  Rejected because: Evidence shows direct console.error usage without structured logging imports or configuration, indicating preference for minimal dependencies
  When valid: When log aggregation, alerting, and correlation across distributed services become operational requirements
- Implement custom error tracking service integration (rejected)
  Rejected because: No evidence of third-party error tracking service imports or configuration in the detected pattern
  When valid: When production error monitoring, user impact tracking, and automated alerting justify the operational complexity
- Silent error handling with fallback values only (rejected)
  Rejected because: Evidence explicitly shows console.error calls, indicating intentional error visibility rather than silent failure
  When valid: Never recommended for server-side operations requiring debugging and monitoring

## Risks

- Console error logs may expose sensitive data from exception objects containing secrets, tokens, or user information
  Mitigation: Implement error sanitization before logging, ensuring exception objects do not contain values from security.secrets_handling sources
  Owner: engineering team
- Lack of structured logging limits production error monitoring and automated alerting capabilities
  Mitigation: Establish log aggregation pipeline that parses console output and correlates errors with request context
  Owner: engineering team
- Console logging in high-throughput async operations may impact performance or create log volume issues
  Mitigation: Monitor log volume and implement sampling or rate limiting if error rates exceed operational thresholds
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
- Wrap all async operations that interact with external services, cache layers, or cookies in try-catch blocks with console.error logging in the catch clause
- Preserve the original exception object when logging to maintain stack traces and error metadata for debugging
- Consider adding contextual information to error logs such as operation type, cart ID, or merchandise ID to aid in debugging distributed async workflows

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis or linting verification to confirm all async server actions contain try-catch blocks with error logging
- Discover and run the project's test suite to verify error logging behavior in cart operations and external API client functions
- Discover and inspect the project's server action exports to confirm console.error usage in exception handling paths

Accept when:
- All async server actions performing cart mutations, external API calls, or webhook processing contain try-catch blocks with console.error logging
- Error log statements preserve the original exception object to maintain stack traces
- Static analysis or linting verification passes without violations of error logging requirements

## Enforcement

- Verified by: Code review verification that async server actions contain try-catch blocks with console.error logging
- Verified by: Static analysis or linting rules detecting missing error handling in async functions
- Verified by: Test coverage verification for error paths in cart operations and external API clients
- Violation handling: Code review rejection for async server actions missing error logging in exception handlers
- Violation handling: Static analysis failures block merge until error handling is added
- Violation handling: Production incidents without error logs trigger retrospective and remediation
- Exception process: Document justification for omitting error logging in specific async operations
- Exception process: Obtain approval from technical lead for exception cases
- Exception process: Record exception in architecture decision log with rationale and review date