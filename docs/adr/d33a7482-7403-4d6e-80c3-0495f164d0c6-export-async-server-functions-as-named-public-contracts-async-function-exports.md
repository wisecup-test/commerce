# Export Async Server Functions as Named Public Contracts: Async Function Exports

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase exports async functions as named public contracts across components, actions, and page modules, establishing a consistent interface boundary pattern.
- Server-side rendering and server actions require async function signatures to coordinate data fetching, cart operations, and metadata generation at request time.
- The pattern separates public API surface (exported async functions) from internal implementation details, enabling modular composition and parallel execution.
- Framework conventions for server components, route handlers, and metadata generation mandate async function exports with specific naming patterns.
- The evidence shows 11 files exporting async functions including addItem, removeItem, updateItemQuantity, Image, SearchPage, RootLayout, Footer, and metadata generators.

## Problem Statement

Components and modules need a consistent paradigm for exposing server-side operations that coordinate async I/O, state mutations, and rendering while maintaining clear contract boundaries between public API surface and internal implementation.

## Decision

1. MAY: Async function exports MAY accept Promise parameters to enable parallel data fetching and avoid waterfall request patterns.

## Policy Block

- MAY Async function exports MAY accept Promise parameters to enable parallel data fetching and avoid waterfall request patterns.

In scope:
- Server action modules that mutate application state
- Page components that perform server-side data fetching
- Metadata generation functions
- Layout components that coordinate async data loading
- API route handlers that interact with external services

Out of scope:
- Client-side event handlers and UI interaction logic
- Pure utility functions without I/O operations
- Type definitions and interface declarations
- Static configuration objects and constants

## Rationale

- The evidence shows 11 files consistently exporting async functions as public contracts, with 87.55% confidence across cart actions, page components, and metadata generators.
- Async function exports enable the framework to coordinate server-side rendering, data fetching, and state mutations at request time while maintaining clear module boundaries.
- Named exports establish explicit contract surfaces that support parallel execution, type safety, and modular composition across the application architecture.
- The pattern separates public API surface from internal implementation, enabling independent evolution of contract signatures and internal logic.

## Consequences

Positive:
- Clear contract boundaries between public API surface and internal implementation enable modular composition and independent testing.
- Async function signatures enable the framework to optimize parallel data fetching and avoid request waterfalls.
- Named exports provide explicit import paths that improve discoverability and support static analysis tools.
- Consistent async patterns across server actions, page components, and metadata generators reduce cognitive load and improve maintainability.

Negative:
- Async function signatures increase complexity for developers unfamiliar with server-side rendering paradigms and async coordination patterns.
- Named exports require explicit import statements, increasing verbosity compared to default export patterns.
- Async boundaries may obscure error propagation and make debugging more difficult without proper error handling conventions.
- The pattern couples module structure to framework conventions, reducing portability to environments without server-side rendering support.

## Alternatives

- Use default exports for all public contracts instead of named exports (rejected)
  Rejected because: Default exports obscure contract boundaries and prevent multiple public functions from being exported from a single module, limiting modularity.
  When valid: Single-responsibility modules that export exactly one public function and have no plans for extension.
- Expose synchronous wrapper functions that internally coordinate async operations (rejected)
  Rejected because: Synchronous wrappers prevent the framework from optimizing parallel data fetching and force sequential execution patterns that degrade performance.
  When valid: Client-side code that must integrate with synchronous event handlers or legacy APIs.
- Use class-based exports with async methods instead of standalone async functions (rejected)
  Rejected because: Class-based patterns introduce additional complexity and state management concerns without providing clear benefits for stateless server operations.
  When valid: Modules that require instance state, lifecycle management, or complex inheritance hierarchies.

## Risks

- Async function contracts may proliferate without clear naming conventions, leading to inconsistent API surface and discoverability issues.
  Mitigation: Establish naming conventions for async exports based on operation type: actions use imperative verbs, page components use descriptive nouns, metadata generators follow framework conventions.
  Owner: engineering team
- Error handling within async function exports may be inconsistent, causing unhandled promise rejections or silent failures.
  Mitigation: Implement standard error handling patterns within async exports, including logging, user-facing error messages, and graceful degradation strategies.
  Owner: engineering team
- Async function boundaries may obscure performance bottlenecks and sequential execution patterns that could be parallelized.
  Mitigation: Use observability tools to trace async execution paths and identify opportunities for parallel data fetching and request optimization.
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
- Establish naming conventions for async exports: server actions use imperative verbs describing state mutations, page components use descriptive nouns matching route structure, metadata generators follow framework-mandated function names.
- Implement consistent error handling within async exports: wrap external service calls in try-catch blocks, log errors with context, return user-facing error messages or fallback values, and coordinate cache invalidation on failure.
- Separate public async function exports from internal implementation by delegating to helper functions for data access, transformation, and external service coordination, enabling independent testing and evolution.

## Continuation Context


Verify commands:
- Discover the project's module system configuration and identify all files exporting async functions; verify that public contracts use named exports with descriptive function names.
- Locate the project's type checking configuration and run type validation to confirm async function signatures match expected contract types.
- Identify the project's testing framework and execute test suites covering async function exports to verify error handling, cache coordination, and state mutation behavior.

Accept when:
- All public API contracts performing server-side operations are exported as async functions with named exports.
- Type checking passes without errors for all async function signatures and their usage sites.
- Test suites covering async exports demonstrate proper error handling, cache invalidation, and state coordination.

## Enforcement

- Verified by: Static analysis tools scan module exports to identify async functions and verify named export patterns.
- Verified by: Code review process checks that new async function exports follow naming conventions and implement consistent error handling.
- Verified by: Type checking in continuous integration validates async function signatures against expected contract types.
- Violation handling: Static analysis failures block pull request merges until async export patterns are corrected.
- Violation handling: Code review feedback requires revision of async function exports that violate naming conventions or error handling standards.
- Violation handling: Type checking errors prevent deployment until async function signatures are corrected.
- Exception process: Exceptions for non-standard async export patterns require architectural review and documentation of rationale.
- Exception process: Legacy modules may temporarily use alternative patterns with a documented migration plan and timeline.
- Exception process: Framework-mandated patterns that conflict with this ADR take precedence and are documented as exceptions.