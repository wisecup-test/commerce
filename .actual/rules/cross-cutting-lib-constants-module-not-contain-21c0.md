---
version: 2
name: cross-cutting-lib-constants-module-not-contain-21c0
description: "Adoption of lib/constants Internal Module for Centralized Application Invariants: Lib Constants Module Not Contain Stateful"
metadata:
  actual:
    adr_id: 21c069f6-0821-4422-8e50-fd8dbb88612c
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

# Adoption of lib/constants Internal Module for Centralized Application Invariants: Lib Constants Module Not Contain Stateful

Scope: applies to `app/product/**`, `app/search/**`, `components/cart/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for:
- Application source modules defining or consuming cross-cutting constants, configuration parameters, or shared domain invariants.
- Components, route handlers, server actions, and service client definitions requiring access to invariant values.

### Rules

- **R-LM-001** SHOULD_NOT: The lib/constants module SHOULD NOT contain stateful logic, mutable structures, or external network invocations.

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
