---
version: 2
name: cross-cutting-modules-requiring-general-purpose-helper-f94d
description: "Adoption of lib/utils Internal Shared Module: Modules Requiring General Purpose Helper Functions"
metadata:
  actual:
    adr_id: f94d6a3a-1e31-407a-98dc-9f3dde128650
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

# Adoption of lib/utils Internal Shared Module: Modules Requiring General Purpose Helper Functions

Scope: applies to `app/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for application route handlers, domain integration clients, and user interface components requiring shared utility routines.

### Rules

- **R-LIB-001** MUST: Modules requiring general-purpose helper functions MUST import them from the shared lib/utils module rather than reimplementing ad-hoc utility logic locally.

### Verify

```bash
# Discover and execute the project static analysis and type verification script
# Discover and execute the project test runner to validate that utility functions pass all unit tests
```

**Accept when:**
- All consuming modules resolve imports from lib/utils with zero compilation or static analysis diagnostics.
- All automated unit tests covering functions in lib/utils pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>