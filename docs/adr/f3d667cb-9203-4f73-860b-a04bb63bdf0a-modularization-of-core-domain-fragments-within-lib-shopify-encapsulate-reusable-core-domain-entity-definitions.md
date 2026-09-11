# Modularization of Core Domain Fragments within `lib/shopify`: Encapsulate Reusable Core Domain Entity Definitions

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The `lib/shopify` module requires a consistent approach to defining and reusing core domain entities.
- Multiple operations (e.g., queries, mutations) often depend on the same underlying data structures or GraphQL fragments.
- Centralizing these definitions improves maintainability and reduces duplication.
- The current codebase demonstrates a pattern of encapsulating shared logic in `fragments` modules.

## Problem Statement

Ensuring consistent and reusable definitions for core domain entities across various operations within the `lib/shopify` module to avoid duplication and maintain architectural clarity.

## Decision

1. MUST: Encapsulate reusable core domain entity definitions, such as GraphQL fragments or shared types, within dedicated `fragments` modules.

## Policy Block

- MUST Encapsulate reusable core domain entity definitions, such as GraphQL fragments or shared types, within dedicated `fragments` modules.

In scope:
- Modules within the `lib/shopify` directory that define or consume core domain entities.
- Files responsible for GraphQL queries or mutations related to specific domain objects.

Out of scope:
- Modules outside the `lib/shopify` directory.
- Utility modules that do not define core domain entities.

## Rationale

- Centralizing core domain entity definitions in `fragments` modules promotes consistency and reduces redundancy across dependent modules.
- This modular approach simplifies updates and refactoring of shared data structures or GraphQL fragments.
- The observed pattern in `lib/shopify/queries/cart.ts` and `lib/shopify/mutations/cart.ts` demonstrates the practical benefits of this structure.

## Consequences

Positive:
- Improved code maintainability and readability.
- Reduced risk of inconsistencies in domain entity definitions.
- Easier onboarding for new developers due to a clear module structure.

Negative:
- Potential for over-fragmentation if not applied judiciously.
- Requires discipline to ensure `fragments` modules remain focused on definitions.

## Alternatives

- Inline definitions within each consuming module. (rejected)
  Rejected because: Leads to code duplication and inconsistencies, making maintenance difficult.
  When valid: For very simple, single-use definitions that have no potential for reuse.
- Global utility files for all shared definitions. (rejected)
  Rejected because: Can lead to large, unmanageable files and unclear ownership, reducing discoverability and increasing coupling.
  When valid: For truly generic, framework-level utilities that are not tied to a specific domain.

## Risks

- `fragments` modules become dumping grounds for unrelated logic.
  Mitigation: Regular code reviews and adherence to the "definitions only" principle.
  Owner: Engineering team
- Over-modularization leading to excessive file creation.
  Mitigation: Establish clear guidelines for what constitutes a "core domain entity" worthy of its own fragment module.
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
- New `fragments` modules should be created alongside their related domain modules (e.g., `cart/fragments.ts` for `cart/queries.ts` and `cart/mutations.ts`).
- Ensure that imports from `fragments` modules are explicit and avoid circular dependencies.

## Continuation Context


Verify commands:
- Inspect the module import graph for `lib/shopify` to confirm adherence to the `fragments` module pattern.
- Run static analysis tools configured to detect unapproved import patterns within the `lib/shopify` directory.
- Execute the project's test suite to ensure no regressions from refactoring or new implementations following this pattern.

Accept when:
- New domain entity definitions are consistently placed in dedicated `fragments` modules.
- All consuming modules correctly import from these `fragments` modules.
- No circular dependencies are introduced by the modularization.

## Enforcement

- Verified by: Automated CI checks for import patterns.
- Verified by: Code reviews for new or modified modules within `lib/shopify`.
- Violation handling: Automated CI failures for non-compliant code.
- Violation handling: Code review comments requiring adherence to the pattern before merging.
- Exception process: Exceptions require explicit approval from a lead architect or designated module owner, documented with a clear rationale.