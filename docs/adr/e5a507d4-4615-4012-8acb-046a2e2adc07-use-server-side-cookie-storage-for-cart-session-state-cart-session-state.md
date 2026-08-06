# Use Server-Side Cookie Storage for Cart Session State: Cart Session State

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The application coordinates cart operations across multiple server-side action handlers that require persistent session state between requests.
- Cart identifiers must survive page navigation and form submissions without client-side state management or URL parameters.
- The runtime environment provides server-side cookie access through async APIs that separate read and write operations from the request/response cycle.
- External service integration requires stable cart identifiers that persist across multiple API calls within a user session.

## Problem Statement

Server-side cart operations need a mechanism to persist cart identifiers across requests without exposing session state to the client or coupling cart lifecycle to URL parameters, while maintaining compatibility with async server action patterns.

## Decision

1. MUST: Cart session state persistence MUST use server-side cookie storage accessed through the runtime's async cookie API.

## Policy Block

- MUST Cart session state persistence MUST use server-side cookie storage accessed through the runtime's async cookie API.

In scope:
- Server-side action handlers for cart operations including add, remove, update, and checkout
- Cart creation workflows that establish new session state
- Cart retrieval operations that depend on persisted identifiers
- Webhook handlers that validate requests using header inspection

Out of scope:
- Client-side cart state management or local storage
- Static page generation or build-time cart operations
- Cart operations that do not require session persistence
- Authentication or user identity management

## Rationale

- The IR evidence shows consistent use of async cookie APIs for cart identifier persistence across two files with 89.55% confidence, indicating an established architectural pattern.
- Server-side cookie storage isolates session state from client manipulation while maintaining compatibility with the async server action concurrency model detected in the evidence.
- The pattern coordinates with external service boundaries where stable cart identifiers enable stateful interactions with third-party APIs across multiple requests.
- Console error logging provides observable failure diagnostics without coupling cart operations to structured logging infrastructure.

## Consequences

Positive:
- Cart session state remains server-controlled and protected from client-side tampering or inspection.
- Async cookie APIs integrate naturally with server action concurrency patterns without blocking request handling.
- Cart identifiers persist across navigation and form submissions without URL pollution or client-side storage complexity.
- The pattern supports stateful external service integration with stable session identifiers.

Negative:
- Async cookie operations introduce await points that complicate control flow and error handling in cart actions.
- Cookie-based session state requires careful management of expiration, domain scope, and security attributes.
- Server-side cookie access couples cart operations to runtime environments that provide async cookie APIs.
- Debugging cart session issues requires server-side log inspection rather than client-side developer tools.

## Alternatives

- Store cart identifiers in URL parameters or query strings (rejected)
  Rejected because: URL-based state exposes cart identifiers to client manipulation, complicates navigation, and creates shareable links with embedded session state that violates security boundaries.
  When valid: Stateless cart operations where cart identifiers are intentionally shareable and do not represent authenticated sessions.
- Use client-side local storage or session storage for cart state (rejected)
  Rejected because: Client-side storage prevents server actions from accessing cart state without additional client-to-server communication and exposes session identifiers to client-side JavaScript.
  When valid: Client-rendered applications where cart operations execute entirely in the browser without server-side action handlers.
- Embed cart state in server-side session stores with opaque session tokens (deferred)
  Rejected because: Not rejected; deferred pending evaluation of session store infrastructure requirements and operational complexity versus cookie-based approach.
  When valid: Applications with existing session store infrastructure or requirements for complex session data beyond cart identifiers.

## Risks

- Cookie size limits or domain restrictions may prevent cart identifier storage in constrained environments.
  Mitigation: Validate cookie storage capacity during deployment and implement fallback mechanisms or error handling for cookie write failures.
  Owner: engineering team
- Async cookie API failures or timeouts may leave cart operations in inconsistent states without persisted identifiers.
  Mitigation: Implement comprehensive error handling with console logging and transaction-like semantics that roll back cart operations on cookie write failures.
  Owner: engineering team
- Cookie-based session state may not survive cross-domain navigation or third-party cookie restrictions in privacy-focused browsers.
  Mitigation: Document cookie domain and path configuration requirements and test cart persistence across expected navigation patterns and browser privacy settings.
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
- Locate the async cookie API in the runtime's server-side module exports and confirm the API surface supports both get and set operations with awaitable promises.
- Wrap all cookie write operations in try-catch blocks with console error logging to ensure cart operation failures are observable in server logs.
- Establish cookie naming conventions and expiration policies that align with cart session lifecycle requirements and coordinate with any existing session management patterns in the codebase.

## Continuation Context


Verify commands:
- Discover the project's test suite location and execute server-side action tests that validate cart identifier persistence across simulated requests.
- Locate the project's linting or static analysis configuration and run checks that enforce async function declarations for cart action handlers.
- Identify the project's integration test framework and run tests that verify cookie storage behavior under error conditions and API failures.

Accept when:
- All cart action handlers successfully persist cart identifiers to cookies after cart creation and retrieve them before cart operations.
- Error handling tests demonstrate that cookie write failures are logged to console error stream and propagate exceptions appropriately.
- Integration tests confirm cart session state survives navigation and form submissions across multiple requests.

## Enforcement

- Verified by: Automated test suites that validate cookie persistence behavior in cart action handlers.
- Verified by: Code review checklists that verify async cookie API usage and error handling patterns.
- Verified by: Integration tests that exercise cart operations across multiple requests with cookie inspection.
- Violation handling: CI pipeline failures block merges when cart action tests fail to demonstrate proper cookie persistence.
- Violation handling: Code review feedback requires revision of cart operations that bypass async cookie APIs or lack error handling.
- Violation handling: Runtime monitoring alerts on elevated console error rates from cart operations indicating cookie storage failures.
- Exception process: Document technical justification for alternative session state mechanisms with security and consistency analysis.
- Exception process: Obtain architecture review approval for cart operations that cannot use server-side cookie storage due to runtime constraints.
- Exception process: Record exceptions in architecture decision log with expiration dates and migration plans to standard cookie-based approach.