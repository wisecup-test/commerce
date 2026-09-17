# Modularization of Query Fragments within `lib/shopify`: Use Relative Imports Fragments Modularizing Reusable

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The project utilizes a `lib/shopify` module for interacting with Shopify data.
- Within this module, query definitions for different entities (e.g., pages, collections) are present.
- These query definitions frequently share common data structures or query parts.
- A convention has emerged to define these reusable parts in separate "fragment" files.

## Problem Statement

To maintain consistency, promote reuse, and simplify complex query definitions within the `lib/shopify` module, a standardized approach for modularizing common query components is required.

## Decision

1. MUST: MUST use relative imports to `../fragments/...` for modularizing reusable query parts or data structures within the `lib/shopify` module.

## Policy Block

- MUST MUST use relative imports to `../fragments/...` for modularizing reusable query parts or data structures within the `lib/shopify` module.

In scope:
- Files within the `lib/shopify` module that define data queries or structures.
- New query definitions or modifications to existing ones that require reusable components.

Out of scope:
- Files outside the `lib/shopify` module.
- Query definitions that do not involve reusable fragments.

## Rationale

- Encourages code reuse and reduces duplication across similar query definitions.
- Improves maintainability by centralizing common data structure definitions.
- Enhances readability of complex queries by abstracting common parts into fragments.
- The observed pattern in `lib/shopify/queries/page.ts` and `lib/shopify/queries/collection.ts` demonstrates an existing successful application of this modular approach.

## Consequences

Positive:
- Increased consistency in query definitions.
- Reduced bundle size due to less redundant code.
- Faster development of new queries by composing existing fragments.
- Easier to update common data structures across the application.

Negative:
- Potential for increased file count if fragments are overly granular.
- Requires developers to understand the fragment structure when navigating the codebase.

## Alternatives

- Inline all query parts directly within each query definition. (rejected)
  Rejected because: Leads to significant code duplication and reduced maintainability, especially for complex queries.
  When valid: For very simple, one-off queries with no reusable parts.
- Use a different modularization strategy (e.g., a dedicated utility file for query builders). (rejected)
  Rejected because: The current evidence shows a clear preference for file-based fragments, and introducing a new strategy would deviate from the established pattern without clear benefits.
  When valid: If the complexity of query construction requires more advanced programmatic composition than simple fragment imports.

## Risks

- Over-fragmentation leading to excessive file navigation.
  Mitigation: Establish guidelines for fragment granularity, focusing on logical units of reuse.
  Owner: engineering team
- Fragments becoming too large or complex, defeating the purpose of modularity.
  Mitigation: Regularly review fragment definitions for cohesion and single responsibility.
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
- New fragments should be placed in a `fragments` directory relative to their consuming queries.
- Fragment names should clearly indicate their purpose (e.g., `productFragment.ts`, `seoFragment.ts`).

## Continuation Context


Verify commands:
- Discover the project's linting configuration and execute the linter across the `lib/shopify` module.
- Discover the project's test suite and execute all tests related to `lib/shopify` queries.
- Discover the project's build command and execute a full build of the application.

Accept when:
- The linter reports no errors or warnings related to fragment imports or definitions.
- All tests within the `lib/shopify` module pass successfully.
- The application builds without errors.

## Enforcement

- Verified by: Code reviews, automated CI checks.
- Violation handling: Code will be rejected during code review or fail CI/CD pipeline.
- Exception process: Requires explicit approval from a lead engineer or architectural review board, with documented rationale for deviation.