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

These rules are ALWAYS ACTIVE for application source modules defining or consuming cross-cutting constants, configuration parameters, or shared domain invariants.

### Rules

- **R-CONST-001** SHOULD_NOT: The lib/constants module SHOULD NOT contain stateful logic, mutable structures, or external network invocations.

### Verify

```bash
# Discover and execute project static analysis checks
# Discover and execute the test runner to validate module resolution and constant imports
```

**Accept when:**
- Static analysis validates that all shared constants and invariants are imported from the centralized lib/constants module without unresolved references.
- All test suites pass with no duplicate invariant definitions across presentation components, route handlers, and server actions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated verification checks module imports and detects duplicated constant literals.
</enforcement>