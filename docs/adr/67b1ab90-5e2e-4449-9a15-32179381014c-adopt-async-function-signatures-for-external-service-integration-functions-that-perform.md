# Adopt Async Function Signatures for External Service Integration: Functions That Perform

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all runtime execution contexts that interact with external services or perform I/O operations.

## Context

- The codebase integrates with external services requiring network I/O, as evidenced by fetch operations to external endpoints with headers and JSON payloads
- Runtime configuration is sourced from environment variables accessed via process.env, requiring asynchronous resolution patterns in server contexts
- Server-side functions access runtime request context through async APIs for cookies and headers, necessitating async function signatures throughout the call chain
- Image generation and file system operations require async I/O patterns for reading font files and constructing responses

## Problem Statement

External service integration and runtime environment access require non-blocking I/O patterns to prevent thread blocking and enable concurrent request handling. Without consistent async function signatures, the system cannot efficiently coordinate multiple I/O operations, access runtime context, or maintain responsive service boundaries.

## Decision

1. MUST: All functions that perform external service calls, file system I/O, or access runtime request context MUST use async function signatures returning Promise types

## Policy Block

- MUST All functions that perform external service calls, file system I/O, or access runtime request context MUST use async function signatures returning Promise types

In scope:
- All functions performing HTTP fetch operations to external service endpoints
- All functions accessing runtime request context including cookies and headers
- All functions performing file system read operations
- All public API contracts exported for service integration
- All functions constructing responses that depend on async I/O operations

Out of scope:
- Pure utility functions performing synchronous data transformations
- Type guard functions that operate on in-memory values
- Constant declarations and static configuration objects
- Synchronous validation logic that does not access external state

## Rationale

- The evidence shows consistent use of async function signatures for shopifyFetch, createCart, addToCart, removeFromCart, updateCart, and OpengraphImage, indicating a deliberate pattern for I/O-bound operations
- Runtime context access through cookies() and headers() requires await, demonstrating that the framework's async APIs necessitate async propagation through the call chain
- External service integration via fetch with JSON payloads and custom headers represents non-blocking I/O that must not block the event loop
- File system operations for font loading use async readFile patterns, confirming the architectural preference for non-blocking I/O across all resource access

## Consequences

Positive:
- Non-blocking I/O enables concurrent handling of multiple requests without thread pool exhaustion
- Async function signatures provide explicit contracts indicating I/O operations, improving code clarity and maintainability
- Promise-based coordination allows parallel execution of independent async operations, reducing total latency
- Framework-provided async APIs for runtime context integrate seamlessly with async function signatures throughout the application

Negative:
- Async function signatures increase syntactic complexity and require developers to understand Promise semantics and error propagation
- Debugging async code paths can be more challenging due to non-linear execution flow and stack trace fragmentation
- Mixing async and sync code requires careful attention to avoid unhandled Promise rejections or forgotten await statements
- Testing async functions requires additional setup for Promise resolution and may increase test execution time

## Alternatives

- Use synchronous blocking I/O for all external service calls and file system operations (rejected)
  Rejected because: Synchronous blocking I/O would block the event loop during network operations, preventing concurrent request handling and degrading system responsiveness under load
  When valid: Only valid for single-threaded batch processing scripts with no concurrency requirements
- Use callback-based asynchronous patterns instead of Promise-based async/await (rejected)
  Rejected because: Callback patterns lead to nested callback pyramids, complicate error handling, and lack the composability and readability of async/await syntax
  When valid: Valid only when integrating with legacy libraries that exclusively provide callback-based APIs
- Isolate all async operations in a dedicated service layer with synchronous facades (deferred)
  When valid: May be valid for architectures requiring strict separation between I/O and business logic, but requires additional abstraction layers

## Risks

- Developers may forget await keywords, causing functions to return unresolved Promises instead of values, leading to runtime errors
  Mitigation: Enable static analysis rules that detect missing await on Promise-returning functions and require explicit Promise handling
  Owner: engineering team
- Unhandled Promise rejections from async operations may cause silent failures or process termination
  Mitigation: Implement global unhandled rejection handlers and ensure all async functions have appropriate try-catch blocks or error boundaries
  Owner: engineering team
- Excessive async function nesting may degrade performance due to Promise overhead for operations that could be synchronous
  Mitigation: Profile async operation overhead and keep pure computation functions synchronous, only using async signatures where I/O actually occurs
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
- When implementing new service integration functions, declare the function signature as async and ensure all I/O operations within the function body use await for Promise resolution
- For functions that access runtime request context, await the context accessor functions before extracting values, and propagate the async signature to all callers in the chain
- When refactoring existing synchronous code to async patterns, audit all call sites to ensure they properly await the function and handle potential Promise rejections

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the linting rules that detect missing await keywords on Promise-returning expressions
- Locate the project's test suite and execute integration tests that verify async service integration functions properly await external calls and handle errors
- Inspect the runtime framework's documentation to identify all async context APIs and verify that application code consistently awaits their resolution

Accept when:
- All functions performing external service calls, file system I/O, or runtime context access use async function signatures
- Static analysis reports zero violations for missing await keywords on Promise-returning function calls
- Integration tests successfully verify that async operations complete and return resolved values rather than pending Promises

## Enforcement

- Verified by: Static analysis tools configured to detect missing await keywords and unhandled Promise rejections
- Verified by: Code review process verifying that new service integration functions use async signatures
- Verified by: Automated test suite validating that async operations properly resolve and handle errors
- Violation handling: Static analysis failures block merge until missing await keywords are added or Promise handling is corrected
- Violation handling: Code review identifies synchronous blocking I/O in request paths and requires refactoring to async patterns
- Violation handling: Runtime monitoring alerts on unhandled Promise rejections, triggering incident response and code correction
- Exception process: Exceptions for synchronous signatures require architectural review demonstrating that the function performs no I/O and is pure computation
- Exception process: Legacy integration code may temporarily use synchronous patterns with documented technical debt and migration plan
- Exception process: Exception approval requires sign-off from technical lead and inclusion in architectural decision log