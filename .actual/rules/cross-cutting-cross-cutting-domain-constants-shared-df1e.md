---
version: 2
name: cross-cutting-cross-cutting-domain-constants-shared-df1e
description: "Adoption of lib/constants Internal Module for Centralized Application Invariants: Cross Cutting Domain Constants Shared Configuration"
metadata:
  actual:
    adr_id: df1ecceb-3fa1-4b7c-9901-869fd9f2237a
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

# Adoption of lib/constants Internal Module for Centralized Application Invariants: Cross Cutting Domain Constants Shared Configuration

Scope: applies to `app/product/**`, `app/search/**`, `components/cart/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for application source modules defining or consuming cross-cutting constants, configuration parameters, or shared domain invariants, including components, route handlers, server actions, and service client definitions.

### Rules

- **R-CONST-001** MUST: All cross-cutting domain constants, shared configuration keys, and application-wide invariants MUST be imported from the internal lib/constants module rather than defined inline or duplicated across components, actions, or route handlers.

### Verify

```bash
# Discover the project verification script defined in the repository configuration and execute the static analysis checks.
# Discover the test runner from the project configuration and execute the test suite to validate module resolution and constant imports.
```

**Accept when:**
- Static analysis validates that all shared constants and invariants are imported from the centralized lib/constants module without unresolved references.
- All test suites pass with no duplicate invariant definitions across presentation components, route handlers, and server actions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>