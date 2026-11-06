---
version: 2
name: cross-cutting-application-components-server-actions-se-8424
description: "Adoption of lib/constants Internal Module for Centralized Application Invariants: Application Components Server Actions Service Integration"
metadata:
  actual:
    adr_id: 84249dc9-20d7-4262-a440-2d187d18709c
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

# Adoption of lib/constants Internal Module for Centralized Application Invariants: Application Components Server Actions Service Integration

Scope: applies to `app/product/**`, `app/search/**`, `components/cart/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONST-001** MUST_NOT: Application components, server actions, and service integration layers MUST NOT redefine local constants that mirror or override declarations present in the lib/constants module.

### Verify

```bash
# Discover the project verification script defined in the repository configuration and execute the static analysis checks.
# Discover the test runner from the project configuration and execute the test suite to validate module resolution and constant imports.
```

**Accept when:**
- Static analysis validates that all shared constants and invariants are imported from the centralized lib/constants module without unresolved references.
- All test suites pass with no duplicate invariant definitions across presentation components, route handlers, and server actions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Automated verification fails if duplicated invariant literals or unauthorized local overrides are detected. Code review blocks merging until shared values are consolidated into the designated internal module.
</enforcement>