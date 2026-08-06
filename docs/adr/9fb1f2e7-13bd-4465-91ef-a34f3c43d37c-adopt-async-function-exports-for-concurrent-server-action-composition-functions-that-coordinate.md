# Adopt Async Function Exports for Concurrent Server Action Composition: Functions That Coordinate

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase exports multiple async functions (addItem, removeItem, updateItemQuantity, redirectToCheckout, createCartAndSetCookie, shopifyFetch, createCart, addToCart, removeFromCart, updateCart) as public API contracts, indicating a pattern of concurrent operation composition.
- Server-side operations coordinate external HTTP clients, cache layer mutations, and cookie/header access through async boundaries, requiring explicit concurrency control at the function export level.
- Error handling uses console.error for logging exceptions within async workflows, establishing a baseline observability pattern for concurrent operation failures.
- The pattern emerges in modules that bridge external service boundaries (Shopify Storefront API) with server-side state management (cookies, cache revalidation), where async composition enables non-blocking I/O.

## Problem Statement

Server actions that coordinate external API calls, cache mutations, and request context access require a consistent concurrency model to prevent blocking operations, ensure proper error propagation, and maintain observability across async boundaries. Without standardized async function exports, concurrent operations risk inconsistent error handling, opaque failure modes, and unpredictable execution ordering.

## Decision

1. MUST: Functions that coordinate multiple async operations (cache writes, external fetches, state mutations) must use await at each async boundary to enforce sequential consistency where order matters.

## Policy Block

- MUST Functions that coordinate multiple async operations (cache writes, external fetches, state mutations) must use await at each async boundary to enforce sequential consistency where order matters.

In scope:
- Server action modules that export functions for external consumption
- Functions performing HTTP requests to external APIs
- Operations mutating server-side cache or revalidation state
- Functions accessing request-scoped context (cookies, headers, search parameters)

Out of scope:
- Pure synchronous utility functions with no I/O
- Client-side event handlers or browser-only code
- Internal helper functions not exported as public contracts
- Type definitions, constants, or configuration objects

## Rationale

- The evidence shows 10 distinct async function exports across two modules (components/cart/actions.ts, lib/shopify/index.ts), demonstrating a consistent pattern of async composition for server-side operations.
- All detected functions coordinate I/O boundaries (fetch calls to external endpoints, cache layer mutations via cookies().set, header/cookie access) where blocking would degrade performance and user experience.
- The presence of console.error(e) within async workflows indicates established error handling expectations that require async/await for proper exception propagation.
- The pattern enables independent concurrent execution of cart operations and Shopify API calls while maintaining sequential consistency within each operation through explicit await points.

## Consequences

Positive:
- Non-blocking I/O operations improve server throughput and response times for concurrent requests.
- Explicit async boundaries make concurrency control visible in function signatures, improving code readability and maintainability.
- Standardized error logging at async boundaries provides consistent observability for debugging concurrent operation failures.
- Async function exports enable parallel composition at the caller level while preserving sequential guarantees within each operation.

Negative:
- Async/await syntax increases cognitive overhead for developers unfamiliar with promise-based concurrency models.
- Error handling requires explicit try-catch blocks at each async boundary, increasing boilerplate code.
- Console-based logging provides limited structured observability compared to dedicated logging frameworks with correlation IDs and trace context.
- Mixing async and sync code paths can introduce subtle bugs if await keywords are omitted or misplaced.

## Alternatives

- Use synchronous function exports with blocking I/O operations (rejected)
  Rejected because: Blocking I/O would serialize all server actions, degrading throughput and user experience under concurrent load. The evidence shows external API calls and cache mutations that require non-blocking execution.
  When valid: Only valid for pure computation with no I/O, which does not match the detected pattern of external service coordination.
- Use callback-based concurrency with explicit continuation passing (rejected)
  Rejected because: Callback-based patterns increase nesting complexity (callback hell) and make error propagation more difficult compared to async/await. Modern runtime environments provide native async/await support.
  When valid: May be valid for legacy codebases or environments without async/await support, but not applicable to the detected server action pattern.
- Adopt a structured logging framework with trace correlation instead of console.error (deferred)
  Rejected because: Not rejected; this is a complementary improvement. The current pattern establishes baseline error logging, but structured logging would enhance observability.
  When valid: Valid as a future enhancement when observability requirements expand beyond basic error logging to include distributed tracing and correlation.

## Risks

- Unhandled promise rejections may cause silent failures if async functions lack try-catch blocks or proper error propagation.
  Mitigation: Enforce linting rules that require error handling in all async functions. Add runtime monitoring for unhandled rejections.
  Owner: engineering team
- Console-based error logging may not provide sufficient context (request IDs, user context, stack traces) for debugging production issues in concurrent environments.
  Mitigation: Evaluate structured logging frameworks that preserve async context and correlation IDs. Document error logging standards in implementation notes.
  Owner: engineering team
- Incorrect await placement or missing await keywords can introduce race conditions or unexpected execution ordering in concurrent operations.
  Mitigation: Use static analysis tools to detect missing await keywords. Require code review for all async function changes with explicit concurrency reasoning.
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
- When implementing new server actions, declare the function signature as async and ensure all I/O operations (fetch, cache access, cookie/header reads) are awaited before proceeding to dependent operations.
- Wrap the entire function body in a try-catch block when the function coordinates external service calls. Log caught errors to console.error with sufficient context (operation name, input parameters) to enable debugging.
- For operations that mutate server-side state (cache writes, cookie sets), await the mutation before returning or proceeding to subsequent operations to ensure state consistency.
- Group related async operations into cohesive modules with clear export boundaries. Use descriptive function names that indicate the async nature and side effects of each operation.

## Continuation Context


Verify commands:
- Discover the project's module analysis tooling and execute static analysis to identify all exported functions in server action modules, verifying that functions performing I/O are declared as async.
- Locate the project's linting configuration and run the linter to detect missing await keywords or unhandled promise rejections in async functions.
- Identify the project's test suite and execute integration tests that verify error logging behavior for async operations under failure conditions.

Accept when:
- All exported functions performing I/O operations (external HTTP, cache mutations, request context access) are declared as async and use await at each async boundary.
- Static analysis confirms no missing await keywords or unhandled promise rejections in async server action functions.
- Integration tests demonstrate that errors in async operations are logged to console.error with sufficient context for debugging.

## Enforcement

- Verified by: Static analysis tools scan for async function declarations and await keyword usage in server action modules.
- Verified by: Code review checklist includes verification of error handling (try-catch blocks) in all async functions that coordinate external services.
- Verified by: Continuous integration pipeline runs linting rules that enforce async/await patterns and detect unhandled promise rejections.
- Violation handling: Linting failures block pull request merges until async/await patterns are corrected.
- Violation handling: Code review identifies missing error handling or incorrect await usage and requests changes before approval.
- Violation handling: Runtime monitoring alerts on unhandled promise rejections in production, triggering incident response and post-mortem analysis.
- Exception process: Exceptions for synchronous function exports require architectural review demonstrating that the operation performs no I/O and has no async dependencies.
- Exception process: Exceptions for alternative error handling patterns (beyond console.error) require documentation of the structured logging approach and correlation strategy.
- Exception process: All exceptions must be documented in code comments with rationale and approval from the engineering lead.