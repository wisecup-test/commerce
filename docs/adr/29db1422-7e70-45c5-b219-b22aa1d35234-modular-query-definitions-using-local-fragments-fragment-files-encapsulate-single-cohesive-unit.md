# Modular Query Definitions Using Local Fragments: Fragment Files Encapsulate Single Cohesive Unit

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- Queries for different data entities (e.g., products, collections) often share common data structures or sub-queries.
- Maintaining consistency and avoiding duplication across these shared query parts is crucial for code quality.
- The codebase utilizes a `shopify` integration layer where data fetching is defined.
- A pattern of importing local "fragment" files has emerged within this layer.

## Problem Statement

Duplication and inconsistency can arise when defining common data structures or sub-queries across multiple data fetching operations, leading to increased maintenance overhead and potential errors.

## Decision

1. SHOULD: Fragment files SHOULD encapsulate a single, cohesive unit of reusable query logic or data shape.

## Policy Block

- SHOULD Fragment files SHOULD encapsulate a single, cohesive unit of reusable query logic or data shape.

In scope:
- Files defining data fetching queries within the `lib/shopify/queries` directory.

Out of scope:
- Files outside the `lib/shopify` directory.
- Files that do not define data fetching queries.

## Rationale

- Centralizing reusable query parts into fragments enhances maintainability by reducing duplication.
- Modularity improves readability and makes it easier to understand the structure of complex queries.
- This pattern promotes consistency in data fetching across different parts of the `shopify` integration.
- The observed usage in `product.ts` and `collection.ts` demonstrates an established practice for managing query complexity.

## Consequences

Positive:
- Reduced code duplication in query definitions.
- Improved maintainability and easier updates to shared query logic.
- Enhanced consistency in data structures across different queries.
- Clearer separation of concerns within the data fetching layer.

Negative:
- Increased number of small files, potentially leading to more file navigation.
- Requires developers to understand the fragment import convention.

## Alternatives

- Inline all query parts directly within each query file. (rejected)
  Rejected because: Leads to significant code duplication and makes maintenance difficult, especially for shared data structures.
  When valid: For very simple queries with no reusable parts, or in contexts where modularity overhead outweighs benefits.
- Define reusable query parts as constants or functions within a shared utility file. (rejected)
  Rejected because: While reducing duplication, it might not offer the same clear structural separation and discoverability as dedicated fragment files, especially for complex GraphQL-like fragments.
  When valid: For very small, non-structural query components or simple string concatenations.

## Risks

- Over-fragmentation leading to an excessive number of small files and increased cognitive load.
  Mitigation: Establish guidelines for fragment granularity, focusing on genuinely reusable and cohesive units.
  Owner: engineering team
- Inconsistent naming or organization of fragment files.
  Mitigation: Define clear naming conventions and directory structures for fragment files.
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
- New fragment files should be placed in a `fragments` directory relative to the query files that consume them.
- Consider using a consistent naming scheme for fragment files (e.g., `productFragment.ts`, `seoFragment.ts`).

## Continuation Context


Verify commands:
- Discover the project's linting configuration and run the linter across the `lib/shopify/queries` directory.
- Discover the project's test runner and execute relevant unit or integration tests for `lib/shopify/queries`.
- Discover the project's build command and perform a full build to ensure no compilation errors related to fragment imports.

Accept when:
- Linter reports no errors related to fragment imports or structure.
- All relevant tests for queries using fragments pass successfully.
- The project builds without errors, and the application functions as expected with modularized queries.

## Enforcement

- Verified by: Code reviews, automated linting, CI/CD pipeline checks.
- Violation handling: Code review comments requiring adherence, automated build failures for linting errors.
- Exception process: Documented exception request process requiring architectural review and approval.