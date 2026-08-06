# Adopt Async Function Exports as Standard Concurrency Model for Server Components and Actions: Server Action Exports

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all server-side component rendering, data fetching, and server action implementations.

## Context

- The codebase exhibits a consistent pattern of exporting async functions as the primary concurrency model across server components, page handlers, image generation endpoints, and server actions
- Evidence shows 11 files implementing async function exports with names like `SearchPage`, `RootLayout`, `Footer`, `Navbar`, `Image`, `OpengraphImage`, `addItem`, `removeItem`, `updateItemQuantity`, `redirectToCheckout`, `shopifyFetch`, `createCart`, `addToCart`, `removeFromCart`, and `updateCart`
- The pattern coordinates server-side data fetching with external service boundaries (Shopify API), cache layers (cookies, headers), and runtime configuration sources (process.env)
- Server actions in cart operations demonstrate async coordination with cache layer mutations, external client calls, and error logging through console.error
- The architecture separates async server-side operations from client-side rendering, with async functions handling data access patterns, external API calls, and file system operations

## Problem Statement

Server-side components, data fetching operations, and server actions require a consistent concurrency model that coordinates asynchronous operations including external API calls, cache layer access, file system operations, and runtime configuration retrieval. Without a standardized approach, the codebase risks inconsistent error handling, unpredictable execution order, and difficulty maintaining server-side data flow patterns.

## Decision

1. MUST: All server action exports that mutate state, interact with cache layers, or call external services MUST be declared as async functions

## Policy Block

- MUST All server action exports that mutate state, interact with cache layers, or call external services MUST be declared as async functions

In scope:
- Server component exports that render pages, layouts, or generate metadata
- Server action exports that handle user interactions requiring state mutations
- Data fetching functions that call external APIs or query services
- Image generation endpoints that read file system resources
- Functions that access cache layers through cookies or headers
- Operations that retrieve runtime configuration from environment sources

Out of scope:
- Client-side component exports that run in browser environments
- Synchronous utility functions that perform pure computations
- Type definitions and interface declarations
- Static configuration objects and constants
- Client-side event handlers that do not trigger server actions

Exceptions:
- EXC-001: A server component wrapper needs to pass an unawaited Promise to a context provider for deferred resolution

## Rationale

- The evidence shows 11 files with 87.55% confidence implementing async function exports as the primary concurrency model, demonstrating a consistent architectural pattern across server-side operations
- Async functions provide native JavaScript concurrency primitives that coordinate multiple asynchronous operations including external API calls to Shopify, cache layer mutations through cookies and headers, and file system access for font loading
- The pattern enables proper sequencing of data dependencies where operations like cart retrieval must complete before cart line manipulation, and configuration loading must precede API client initialization
- Adopting async functions as the standard concurrency model aligns with the observed evidence of server actions (addItem, removeItem, updateItemQuantity), page components (SearchPage, RootLayout), and data fetching operations (shopifyFetch, createCart) all using async/await patterns

## Consequences

Positive:
- Consistent concurrency model across all server-side operations improves code readability and maintainability
- Native async/await syntax provides clear execution order and error propagation through try/catch blocks
- Async functions enable proper coordination of external service calls, cache operations, and file system access without callback nesting
- The pattern supports deferred Promise resolution when passing cart data to context providers, optimizing server-side rendering performance

Negative:
- Async functions introduce execution overhead compared to synchronous operations, though this is negligible for I/O-bound server operations
- Developers must understand Promise semantics and await behavior to avoid unhandled Promise rejections
- Mixing async and sync code patterns requires careful attention to ensure all asynchronous operations are properly awaited
- Error handling becomes more complex as errors can occur at multiple await points requiring comprehensive try/catch coverage

## Alternatives

- Use callback-based concurrency with nested callbacks for asynchronous operations (rejected)
  Rejected because: Callback nesting creates deeply nested code structures that are difficult to read and maintain, and the evidence shows no callback patterns in the detected files
  When valid: Legacy codebases that cannot adopt modern async/await syntax due to runtime constraints
- Use Promise chains with .then() and .catch() for asynchronous flow control (rejected)
  Rejected because: Promise chains are more verbose than async/await and the evidence shows consistent use of async function declarations rather than Promise chain patterns
  When valid: Scenarios requiring dynamic Promise composition where async/await syntax becomes cumbersome
- Use synchronous blocking operations for all server-side data fetching (rejected)
  Rejected because: Synchronous blocking would prevent concurrent request handling and the evidence shows external API calls, cache operations, and file system access all require asynchronous coordination
  When valid: Never valid for server operations that perform I/O or external service calls

## Risks

- Unhandled Promise rejections in async functions may cause silent failures or server crashes if error boundaries are not properly implemented
  Mitigation: Implement comprehensive error handling with try/catch blocks around all await statements and log errors through the established logging mechanism (console.error as shown in evidence)
  Owner: Engineering team
- Async function execution order may become unpredictable if developers do not properly await dependent operations, leading to race conditions in cart operations or cache mutations
  Mitigation: Establish code review guidelines that verify proper await usage for all operations with data dependencies, and implement integration tests that validate execution order
  Owner: Engineering team
- Performance degradation may occur if async operations are unnecessarily serialized when they could execute in parallel
  Mitigation: Document patterns for parallel execution using Promise.all() when operations are independent, and profile server response times to identify serialization bottlenecks
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
- When implementing server actions that mutate cache layers, ensure all cache operations are awaited before proceeding with subsequent logic to prevent race conditions between cache writes and redirects
- For server components that pass Promise objects to context providers, document the deferred resolution pattern and verify that downstream consumers properly handle the Promise lifecycle
- Implement error logging at async function boundaries using the established logging mechanism to capture failures in external API calls, cache operations, and file system access

## Continuation Context


Verify commands:
- Discover the project's test runner configuration and execute the test suite that validates server component rendering and server action behavior
- Discover the project's static analysis tooling and run type checking to verify all async function exports have proper return type annotations
- Discover the project's linting configuration and execute linting rules that enforce await usage for Promise-returning operations

Accept when:
- All server component exports that perform I/O operations are declared as async functions and properly await asynchronous operations
- All server action exports that mutate state or call external services are declared as async functions with comprehensive error handling
- Type checking passes without errors related to Promise handling or async function return types

## Enforcement

- Verified by: Automated type checking in continuous integration pipeline verifies async function signatures and await usage
- Verified by: Code review process validates that all server-side I/O operations use async/await patterns
- Verified by: Integration tests verify proper execution order and error handling in async server actions
- Violation handling: Type checking failures block pull request merging until async function signatures are corrected
- Violation handling: Code review identifies missing await statements and requires revision before approval
- Violation handling: Integration test failures trigger investigation of async operation sequencing and error handling
- Exception process: Exceptions require tech lead approval with documented justification for deviating from async function pattern
- Exception process: Exception requests must demonstrate that alternative concurrency model is necessary for specific technical constraints
- Exception process: Approved exceptions are documented in code comments explaining the rationale and alternative approach