# Modular Cart Fragments in Shopify Integration: Verify Exact Resolved Version Any Versioned

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The `lib/shopify` module encapsulates integration logic for the Shopify platform.
- Cart-related operations (`mutations`, `queries`) are a significant part of this integration.
- To maintain consistency and reduce code duplication, shared components for cart functionality are centralized.
- The codebase exhibits a pattern of importing shared cart logic from a dedicated `fragments/cart` module.

## Problem Statement

Ensuring consistent and reusable implementation of cart-related data structures and utility functions across different Shopify integration components.

## Decision

1. MUST: Verify the exact resolved version of any versioned dependency before implementation by inspecting the project's lock file.

## Policy Block

- MUST Verify the exact resolved version of any versioned dependency before implementation by inspecting the project's lock file.

In scope:
- Code within the `lib/shopify` directory.
- Components implementing or extending Shopify cart functionality.

Out of scope:
- Code outside the `lib/shopify` directory.
- Shopify integration logic not related to cart functionality.

## Rationale

- Centralizing cart fragments promotes code reuse and reduces redundancy across different cart operations.
- Using a dedicated module improves maintainability by providing a single source of truth for cart-related definitions.
- Consistent import paths enforce a clear dependency structure within the Shopify integration.

## Consequences

Positive:
- Improved code consistency and reduced duplication for cart-related logic.
- Easier maintenance and updates of cart data structures and utility functions.
- Clearer module boundaries within the Shopify integration.

Negative:
- Potential for increased complexity if `fragments/cart` becomes a monolithic module.
- Requires developers to be aware of and adhere to this specific modularization pattern.

## Alternatives

- Duplicate cart-related logic across consuming files. (rejected)
  Rejected because: Leads to code duplication, inconsistency, and increased maintenance overhead.
  When valid: For very small, isolated, and non-reusable pieces of logic that are unlikely to change.
- Define cart fragments directly within each consuming file. (rejected)
  Rejected because: Scatters definitions and makes global changes difficult, similar to duplication.
  When valid: For highly specific, single-use definitions that have no shared applicability.

## Risks

- The `fragments/cart` module becomes too large or contains unrelated logic.
  Mitigation: Regularly review the contents of `fragments/cart` to ensure it remains focused on shared cart components.
  Owner: Engineering Team.

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
- 1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
- 2. Identify the build tool from the manifest.
- 3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
- 4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
- 5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
- 6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- New shared cart-related types or utility functions should be added to `fragments/cart`.
- Existing cart logic in `lib/shopify` should be refactored to use `fragments/cart` if it contains duplicated definitions.

## Continuation Context


Verify commands:
- Inspect the dependency graph for `lib/shopify/mutations/cart.ts` to confirm imports from `fragments/cart`.
- Inspect the dependency graph for `lib/shopify/queries/cart.ts` to confirm imports from `fragments/cart`.
- Run static analysis tools to detect unreferenced or duplicated cart-related logic outside `fragments/cart` within `lib/shopify`.

Accept when:
- `lib/shopify/mutations/cart.ts` and `lib/shopify/queries/cart.ts` exclusively import shared cart components from `fragments/cart`.
- No duplicated cart-related types or utility functions are found within `lib/shopify` outside of `fragments/cart`.

## Enforcement

- Verified by: Code reviews
- Verified by: Automated static analysis
- Violation handling: Code will be rejected during code review; automated checks will fail CI/CD pipelines.
- Exception process: Exceptions require explicit approval from the architecture review board, documented with a clear rationale and alternative solution.