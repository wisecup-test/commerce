# Adopt Server-Side Cookie-Based Cart Identifier Persistence: Cart Identifier Persistence

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The application implements shopping cart functionality requiring persistent cart identification across stateless HTTP requests
- Cart operations (addItem, removeItem, updateItemQuantity, redirectToCheckout, createCartAndSetCookie) execute as server actions with asynchronous concurrency semantics
- Cart state synchronization depends on a stable identifier that survives page navigation and session boundaries
- The system integrates with an external commerce platform requiring cart line item queries by merchandise identifier
- Error handling uses console-based logging for cart operation failures

## Problem Statement

Server-side cart operations require a mechanism to associate ephemeral HTTP requests with persistent cart state stored in an external commerce system, while maintaining identifier consistency across navigation events and enabling concurrent cart mutations without identifier collision.

## Decision

1. MUST: Cart identifier persistence MUST use server-accessible cookie storage with asynchronous write semantics

## Policy Block

- MUST Cart identifier persistence MUST use server-accessible cookie storage with asynchronous write semantics

In scope:
- Server-side cart action handlers
- Cart identifier persistence layer
- Cart state synchronization operations
- Commerce platform integration points

Out of scope:
- Client-side cart state management
- Cart UI rendering logic
- Payment processing workflows
- Inventory validation

## Rationale

- Cookie-based persistence provides server-accessible storage that survives page navigation and enables stateless request handling without client-side state management complexity
- Asynchronous cookie write semantics align with the detected server action concurrency model (addItem, removeItem, updateItemQuantity, redirectToCheckout, createCartAndSetCookie)
- Merchandise identifier-based line item lookup (cart.lines.find with merchandise.id comparison) enables idempotent cart operations and duplicate detection
- The pattern separates cart identifier persistence (cache layer) from cart state storage (external commerce platform), enabling independent scaling and failure isolation

## Consequences

Positive:
- Cart identifier persistence survives page reloads, navigation events, and session boundaries without client-side storage dependencies
- Server-side cookie access enables cart operations to execute within server action contexts without client round-trips
- Merchandise identifier-based queries provide deterministic line item lookup for update and removal operations
- Separation of identifier persistence from cart state storage enables independent cache invalidation and commerce platform migration

Negative:
- Cookie storage introduces latency for asynchronous write operations during cart creation
- Server-side cookie access requires framework-specific APIs that couple cart operations to the server runtime environment
- Console-based error logging provides limited observability for production cart operation failures
- Merchandise identifier-based queries require full cart line collection traversal for each lookup operation

## Alternatives

- Client-side localStorage for cart identifier persistence (rejected)
  Rejected because: Server actions require server-accessible storage; client-side localStorage would necessitate additional request parameters and break server action encapsulation
  When valid: Valid for client-side cart operations that do not require server action isolation
- Session-based cart identifier storage with server-side session management (rejected)
  Rejected because: Session storage introduces stateful server requirements and complicates horizontal scaling; cookie-based approach maintains stateless request handling
  When valid: Valid for applications with existing session infrastructure and single-server deployments
- URL parameter-based cart identifier propagation (rejected)
  Rejected because: URL parameters expose cart identifiers in browser history and require explicit propagation across all navigation events; cookies provide automatic propagation
  When valid: Valid for public cart sharing workflows where URL-based cart access is required

## Risks

- Cookie storage failures during cart creation result in orphaned carts without persistent identifiers
  Mitigation: Implement cart creation retry logic with idempotency guarantees and monitor cookie write success rates
  Owner: engineering team
- Console-based error logging provides insufficient observability for diagnosing production cart operation failures
  Mitigation: Augment console logging with structured error tracking and alerting for cart operation error rates
  Owner: engineering team
- Linear cart line traversal for merchandise identifier lookup degrades performance for large cart collections
  Mitigation: Monitor cart size distributions and implement indexed lookup structures if cart line counts exceed performance thresholds
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
- Cart creation operations must atomically execute commerce platform cart instantiation and cookie persistence within a single transaction boundary to prevent identifier inconsistency
- Implement cart identifier validation on all cart mutation operations to detect and handle missing or invalid cookie values before executing commerce platform API calls
- Consider implementing cart line item lookup optimization using Map-based indexing if cart size metrics indicate performance degradation with linear traversal

## Continuation Context


Verify commands:
- Discover the project's test execution configuration and run the test suite covering cart action handlers to verify cookie persistence behavior
- Discover the project's static analysis tooling and execute type checking to verify cart operation signatures match the public API contracts
- Discover the project's integration test infrastructure and execute commerce platform integration tests to verify cart identifier persistence across operation sequences

Accept when:
- All cart action handler tests pass, demonstrating successful cart identifier persistence and retrieval from cookie storage
- Type checking confirms cart operation signatures expose addItem, removeItem, updateItemQuantity, redirectToCheckout, and createCartAndSetCookie contracts
- Integration tests verify cart operations maintain identifier consistency across multiple mutation sequences

## Enforcement

- Verified by: Automated test suite execution in continuous integration pipeline
- Verified by: Code review verification of cart operation cookie access patterns
- Verified by: Static analysis of cart action handler implementations
- Violation handling: Test failures block merge for cart operations that bypass cookie-based identifier persistence
- Violation handling: Code review rejection for cart actions that implement alternative identifier storage mechanisms
- Violation handling: Static analysis warnings for cart operations missing merchandise identifier-based line item queries
- Exception process: Document architectural justification for alternative identifier persistence mechanisms
- Exception process: Obtain approval from technical lead for cart operation patterns that deviate from cookie-based storage
- Exception process: Record exception rationale in ADR amendment with evidence of performance or security requirements