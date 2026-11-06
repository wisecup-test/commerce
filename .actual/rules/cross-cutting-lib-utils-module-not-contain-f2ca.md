---
version: 2
name: cross-cutting-lib-utils-module-not-contain-f2ca
description: "Adoption of lib/utils Internal Shared Module: Lib Utils Module Not Contain Domain"
metadata:
  actual:
    adr_id: f2ca64f0-12f1-4f28-ba50-763be5368544
    status: accepted
    candidate_group_id: 1fedad58-d6b8-493a-bef9-5cef8b98908b
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
      - components/layout/search/filter/**
      - lib/shopify/**
---

# Adoption of lib/utils Internal Shared Module: Lib Utils Module Not Contain Domain

Scope: applies to `app/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for application route handlers, domain integration clients, and user interface components requiring shared utility routines.

### Rules

- **R-LIB-001** MUST_NOT: The lib/utils module MUST NOT contain domain-specific business logic or client-specific state management that creates circular dependencies with consuming layers.

### Verify

```bash
# Discover and execute the project static analysis and type verification script
# Discover and execute the project test runner to validate that utility functions pass all unit tests
```

**Accept when:**
- All consuming modules resolve imports from lib/utils with zero compilation or static analysis diagnostics.
- All automated unit tests covering functions in lib/utils pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated static analysis checks in CI pipelines and peer code review.
</enforcement>