---
version: 2
name: cross-cutting-module-consumers-discover-repository-loc-d25c
description: "Adoption of lib/constants Internal Module for Centralized Application Invariants: Module Consumers Discover Repository Lock Resolution"
metadata:
  actual:
    adr_id: d25c8992-5c6d-4384-a8e4-2dbe81bb4974
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

# Adoption of lib/constants Internal Module for Centralized Application Invariants: Module Consumers Discover Repository Lock Resolution

Scope: applies to `app/product/**`, `app/search/**`, `components/cart/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONST-001** MUST: Module consumers MUST discover the repository lock or resolution artifact to verify the exact resolved version of all external runtime dependencies prior to implementing or modifying shared module integrations.

### Verify

```bash
# Discover and execute project verification script defined in repository configuration
# Discover and execute test runner to validate module resolution and constant imports
```

**Accept when:**
- Static analysis validates that all shared constants and invariants are imported from the centralized lib/constants module without unresolved references.
- All test suites pass with no duplicate invariant definitions across presentation components, route handlers, and server actions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>