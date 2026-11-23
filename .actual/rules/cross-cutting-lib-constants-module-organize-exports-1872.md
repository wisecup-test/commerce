---
version: 2
name: cross-cutting-lib-constants-module-organize-exports-1872
description: "Adoption of lib/constants Internal Module for Centralized Application Invariants: Lib Constants Module Organize Exports Cohesive"
metadata:
  actual:
    adr_id: 1872643d-186e-441b-a508-818267cb9c11
    status: accepted
    candidate_group_id: b6447dca-b165-4cdd-80f5-5a5fa0295df5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/product/**
      - app/search/**
      - components/cart/**
      - components/layout/search/filter/**
      - lib/shopify/**
---

# Adoption of lib/constants Internal Module for Centralized Application Invariants: Lib Constants Module Organize Exports Cohesive

Scope: applies to `app/product/**`, `app/search/**`, `components/cart/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for:
- Application source modules defining or consuming cross-cutting constants, configuration parameters, or shared domain invariants.
- Components, route handlers, server actions, and service client definitions requiring access to invariant values.

### Rules

- **R-LM-001** SHOULD: The lib/constants module SHOULD organize exports into cohesive domain categories to ensure single-responsibility grouping and modular consumption across client and server boundaries.

### Verify

- Discover the project verification script defined in the repository configuration and execute the static analysis checks.
- Discover the test runner from the project configuration and execute the test suite to validate module resolution and constant imports.

**Accept when:**

- Static analysis validates that all shared constants and invariants are imported from the centralized lib/constants module without unresolved references.
- All test suites pass with no duplicate invariant definitions across presentation components, route handlers, and server actions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis checks verifying module imports and detecting duplicated constant literals.
- Verified by: Peer code review during pull request evaluation.
- Violation handling: Automated verification fails if duplicated invariant literals or unauthorized local overrides are detected.
- Violation handling: Code review blocks merging until shared values are consolidated into the designated internal module.
- Exception process: Architectural review and documented approval justifying local isolation for strictly component-specific private values.
</enforcement>
