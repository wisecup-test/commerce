# Adopt Array.find() for Predicate-Based Collection Queries: Predicate Functions Passed

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase processes in-memory collections where a single element matching a predicate must be retrieved
- Cart line items require lookup by merchandise identifier to support update and removal operations
- Search sorting configuration requires matching user-supplied sort parameters against a predefined list of sort options
- The pattern appears in both server-side action handlers and page rendering logic, indicating a cross-cutting data access concern

## Problem Statement

When querying in-memory collections for a single element matching a predicate, developers need a consistent, readable approach that clearly expresses intent, handles missing elements gracefully, and avoids verbose iteration patterns that obscure business logic.

## Decision

1. MUST: Predicate functions passed to find must use arrow function syntax with explicit parameter names that reflect the domain concept being queried

## Policy Block

- MUST Predicate functions passed to find must use arrow function syntax with explicit parameter names that reflect the domain concept being queried

In scope:
- In-memory array and collection queries within server actions
- Page rendering logic that filters configuration or lookup tables
- Cart line item queries by merchandise identifier
- Sort configuration lookups by slug parameter

Out of scope:
- Database queries or ORM operations
- Queries requiring multiple matching elements
- Stream or iterator-based data sources
- Queries where index position is the primary concern

## Rationale

- The evidence shows consistent use of find with arrow function predicates across cart actions and search page logic, indicating an established pattern for single-element retrieval
- The pattern appears in 2 files with 88.40% confidence, demonstrating deliberate adoption rather than isolated usage
- Using find with inline predicates produces more readable code than imperative loops, making business logic explicit at the call site
- The pattern aligns with functional programming idioms common in modern JavaScript and TypeScript codebases

## Consequences

Positive:
- Code intent is immediately clear: find signals single-element retrieval with a predicate
- Reduced boilerplate compared to imperative loop constructs with early return or break statements
- Type inference works naturally with arrow functions, improving IDE support and type safety
- The pattern is idiomatic in JavaScript/TypeScript ecosystems, reducing cognitive load for developers familiar with functional array methods

Negative:
- Developers unfamiliar with functional array methods may require training on find semantics and undefined handling
- The pattern does not optimize for cases where element position or index is needed alongside the element itself
- Chaining multiple find operations can lead to nested undefined checks if not handled with nullish coalescing
- Performance characteristics differ from imperative loops in edge cases involving very large collections or complex predicates

## Alternatives

- Use imperative for-loops with early return for all single-element queries (rejected)
  Rejected because: Imperative loops obscure intent with boilerplate iteration logic and require explicit return or break statements, reducing readability compared to declarative find calls
  When valid: When index position is required alongside the element, or when performance profiling demonstrates find overhead is unacceptable
- Use filter followed by array indexing to retrieve the first matching element (rejected)
  Rejected because: Filter processes the entire collection even after finding a match, wasting computation, and requires additional indexing syntax that adds verbosity without clarity
  When valid: Never; find is strictly superior for single-element retrieval with short-circuit behavior
- Introduce a custom utility function wrapping find with additional error handling or logging (deferred)
  Rejected because: Not rejected; deferred pending evidence of repeated undefined-handling patterns that justify abstraction
  When valid: When multiple call sites require identical fallback logic, logging, or error handling around find operations

## Risks

- Developers may forget to handle the undefined return value from find, leading to runtime errors when no element matches
  Mitigation: Enforce strict null checks in the type system configuration and include undefined-handling verification in code review checklists
  Owner: engineering team
- Complex predicates with side effects may introduce subtle bugs if developers assume find will process all elements
  Mitigation: Document that find short-circuits on first match and prohibit side effects in predicate functions through linting rules
  Owner: engineering team
- Performance degradation in scenarios with very large collections or computationally expensive predicates
  Mitigation: Profile hot paths and consider indexed data structures or memoization for collections exceeding performance thresholds
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
- When migrating existing imperative loops to find, verify that the original loop did not rely on index position or mutation of external state during iteration
- For collections where no match is expected to be a valid business case, use nullish coalescing or logical OR to provide default values inline rather than separate conditional blocks
- Consider enabling strict null checking in the type system to catch missing undefined handling at compile time rather than runtime

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the linting verification script to ensure all collection queries use find instead of imperative loops
- Locate the project's type checking configuration and run the type verification script with strict null checks enabled to confirm undefined handling is present
- Identify the project's test suite entry point and execute unit tests covering cart actions and search page logic to verify find operations handle both match and no-match cases

Accept when:
- All predicate-based single-element queries in cart actions and search logic use find with arrow function predicates
- Type checking passes with strict null checks enabled and all find results are handled for undefined cases
- Unit tests demonstrate correct behavior for both matching and non-matching predicate scenarios

## Enforcement

- Verified by: Static analysis rules detecting imperative loop patterns where find is applicable
- Verified by: Type system verification with strict null checking enabled
- Verified by: Code review checklist items for predicate function purity and undefined handling
- Violation handling: Linting failures block merge to main branch until imperative loops are refactored to find
- Violation handling: Type errors from missing undefined handling block build pipeline progression
- Violation handling: Code review feedback requires revision before approval when find is not used for applicable queries
- Exception process: Developer documents performance profiling evidence showing find overhead is unacceptable for the specific use case
- Exception process: Tech lead reviews exception request and approves with inline comment explaining why imperative approach is required
- Exception process: Exception is recorded in architecture decision log with reference to profiling data and approval