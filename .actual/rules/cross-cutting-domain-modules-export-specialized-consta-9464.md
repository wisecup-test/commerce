---
version: 2
name: cross-cutting-domain-modules-export-specialized-consta-9464
description: "Adoption of lib/constants Internal Module for Centralized Application Invariants: Domain Modules Export Specialized Constants Through"
metadata:
  actual:
    adr_id: 94643c0a-7620-453a-935a-8d2ef3e7f11f
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

# Adoption of lib/constants Internal Module for Centralized Application Invariants: Domain Modules Export Specialized Constants Through

Scope: applies to `app/product/**`, `app/search/**`, `components/cart/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for application source modules defining or consuming cross-cutting constants, configuration parameters, or shared domain invariants.

### Rules

- **R-CONST-001** MAY: Domain modules MAY re-export specialized constants through scoped namespace wrappers when domain encapsulation is explicitly required.

### Verify

```bash
# Discover the project verification script defined in the repository configuration and execute the static analysis checks.
# Discover the test runner from the project configuration and execute the test suite to validate module resolution and constant imports.
```

**Accept when:**
- Static analysis validates that all shared constants and invariants are imported from the centralized lib/constants module without unresolved references.
- All test suites pass with no duplicate invariant definitions across presentation components, route handlers, and server actions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated verification fails if duplicated invariant literals or unauthorized local overrides are detected.
</enforcement>