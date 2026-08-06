# Adopt Async Function Declarations for Server-Side Data Operations: Page Rendering Functions

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all server-side data operations, API route handlers, and component rendering functions that perform I/O operations.

## Context

- The codebase performs server-side operations including cart management, page rendering, image generation, and external API calls to Shopify services
- Functions such as addItem, removeItem, updateItemQuantity, redirectToCheckout, createCartAndSetCookie, shopifyFetch, createCart, addToCart, removeFromCart, updateCart, OpengraphImage, SearchPage, RootLayout, generateMetadata, Page, Footer, Navbar, and Image are declared as async functions
- Server-side data fetching requires coordination with external services, cookie management, header inspection, cache revalidation, and file system operations
- The framework environment supports async/await patterns for server components, API routes, and data fetching operations
- Concurrency model choices directly impact response latency, error handling patterns, and the ability to compose multiple I/O operations

## Problem Statement

Server-side operations that perform I/O—including external API calls, file system access, cookie and header inspection, and database queries—require a consistent concurrency model that enables non-blocking execution, composable error handling, and sequential or parallel coordination of multiple asynchronous operations. Without a standardized approach, teams may introduce blocking calls, inconsistent error propagation, or difficult-to-maintain callback chains.

## Decision

1. MUST: Page rendering functions, metadata generation functions, and image generation functions that fetch data MUST be declared as async functions.

## Policy Block

- MUST Page rendering functions, metadata generation functions, and image generation functions that fetch data MUST be declared as async functions.

In scope:
- Server-side API route handlers
- Server component rendering functions
- Data fetching functions that call external services
- Cart management actions
- Metadata and Open Graph image generation functions
- Functions that access cookies, headers, or request context
- File system operations for font loading or asset access

Out of scope:
- Client-side event handlers
- Pure computation functions with no I/O
- Synchronous utility functions
- Type definitions and interfaces
- Static configuration objects

## Rationale

- The evidence shows 11 files with async function declarations for server-side operations, indicating a consistent pattern across cart actions, page rendering, image generation, and API integration
- Async/await syntax provides linear control flow for sequential operations while enabling parallel execution of independent tasks, improving both code readability and runtime performance
- The framework's server-side execution model supports Promise-based concurrency, allowing functions to coordinate multiple I/O operations without blocking the event loop
- Adopting async functions as the standard concurrency model ensures consistent error handling, simplifies testing, and enables composition of complex data fetching workflows

## Consequences

Positive:
- Non-blocking I/O operations improve server throughput and response times
- Async/await syntax provides clear, sequential code structure that is easier to read and maintain than callback-based patterns
- Parallel execution of independent operations reduces total latency for complex page renders or API compositions
- Consistent error handling patterns across all async operations simplify debugging and monitoring

Negative:
- Async functions introduce Promise overhead and require careful management of error propagation
- Developers must understand Promise semantics, event loop behavior, and potential race conditions
- Debugging async code can be more complex due to asynchronous stack traces
- Improper use of await can serialize operations that could run in parallel, degrading performance

## Alternatives

- Use callback-based concurrency with continuation-passing style for all I/O operations (rejected)
  Rejected because: Callback-based patterns lead to deeply nested code structures, make error handling inconsistent, and are harder to compose than Promise-based async/await
  When valid: Legacy codebases that cannot adopt modern async/await syntax
- Use synchronous blocking calls for all I/O operations (rejected)
  Rejected because: Synchronous I/O blocks the event loop, severely degrading server throughput and preventing concurrent request handling
  When valid: Single-threaded CLI tools or scripts where concurrency is not required
- Use reactive streams or observables for all async operations (rejected)
  Rejected because: Reactive patterns add complexity and learning curve without clear benefits for the request-response model used in server-side rendering and API routes
  When valid: Real-time data streaming applications or complex event-driven architectures

## Risks

- Unhandled Promise rejections may cause silent failures or process crashes if error handling is incomplete
  Mitigation: Enforce try-catch blocks around all await statements in critical paths and configure runtime to log unhandled rejections
  Owner: Engineering team
- Excessive serialization of async operations may degrade performance if developers await operations that could run in parallel
  Mitigation: Provide code review guidelines and examples demonstrating parallel execution patterns using Promise.all or Promise.allSettled
  Owner: Engineering team
- Memory leaks or resource exhaustion may occur if long-running async operations are not properly canceled or timed out
  Mitigation: Implement timeout mechanisms for external API calls and ensure cleanup logic runs in finally blocks
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When implementing new server-side functions, declare them as async if they perform any I/O operation, including external API calls, file system access, or runtime context inspection
- For functions that initiate multiple independent async operations, use parallel execution patterns to minimize total latency rather than awaiting each operation sequentially
- Ensure all async functions have appropriate error handling using try-catch blocks or Promise rejection handlers, and log errors with sufficient context for debugging

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the linting tool to verify all I/O functions are declared as async
- Discover the project's test suite and execute tests that validate async function behavior, error handling, and Promise resolution
- Discover the project's type checking configuration and execute the type checker to verify all async functions return Promise types

Accept when:
- All server-side functions that perform I/O operations are declared as async and return Promises
- Static analysis and type checking pass without errors related to async function declarations or Promise handling
- Test suite validates that async operations handle errors correctly and complete successfully

## Enforcement

- Verified by: Static analysis tools configured to flag non-async functions that perform I/O operations
- Verified by: Type checking enforces Promise return types for all async functions
- Verified by: Code review checklist includes verification of async/await usage for I/O operations
- Verified by: Automated tests validate async function behavior and error handling
- Violation handling: Static analysis failures block pull request merging
- Violation handling: Code review identifies and requires correction of synchronous I/O operations
- Violation handling: Runtime monitoring alerts on unhandled Promise rejections
- Exception process: Exceptions require architectural review and documentation of why synchronous execution is necessary
- Exception process: Approved exceptions must be documented in code comments with justification
- Exception process: Exception approval requires sign-off from technical lead