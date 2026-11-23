---
version: 2
name: cross-cutting-modules-not-introduce-alternative-user-92ad
description: "React Core Library Adoption: Modules Not Introduce Alternative User Interface"
metadata:
  actual:
    adr_id: 92ada000-e925-4494-bbdb-a05013752806
    status: accepted
    candidate_group_id: dd4cf757-029b-4c55-a073-09368a803835
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
      - components/**
---

# React Core Library Adoption: Modules Not Introduce Alternative User Interface

Scope: applies to `app/search/**`, `components/**`.

These rules are ALWAYS ACTIVE for:
- User interface components, layout wrappers, and interactive client context modules requiring declarative view rendering and local state management.

### Rules

- **R-LM-001** MUST_NOT: Modules MUST NOT introduce alternative user interface rendering frameworks or imperative view manipulation libraries into the component hierarchy.

### Verify

- Discover and execute the repository type-checking script to validate React component signatures, hook parameter types, and context definitions across all presentation modules.
- Discover and execute the repository static analysis and linting scripts to verify compliance with component rendering directives and hook dependency guidelines.
- Discover and execute the repository test suite to confirm that reactive components and context providers render and transition state as expected.

**Accept when:**

- All component definitions type-check successfully with zero errors across interface modules.
- Static analysis validates that all hook calls conform to dependency constraints without unescaped imperative view mutations.
- Automated test suites pass without regressions across interactive views and state context providers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and type verification executed during continuous integration workflows.
- Verified by: Peer code review validating component boundary placement, hook dependency completeness, and proper client directive usage.
- Violation handling: Build pipelines fail and block code integration if type mismatches, invalid hook usage, or unapproved rendering libraries are detected.
- Violation handling: Code review requires refactoring any imperative view mutations into standard declarative component patterns.
- Exception process: Formal architectural review request documenting technical constraints that necessitate an exception from standard declarative component patterns.
</enforcement>
