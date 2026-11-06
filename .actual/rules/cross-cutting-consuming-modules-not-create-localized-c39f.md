---
version: 2
name: cross-cutting-consuming-modules-not-create-localized-c39f
description: "Adoption of lib/utils Internal Shared Module: Consuming Modules Not Create Localized Duplicate"
metadata:
  actual:
    adr_id: c39f20dc-f194-419f-b349-791f1110349e
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

# Adoption of lib/utils Internal Shared Module: Consuming Modules Not Create Localized Duplicate

Scope: applies to `app/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for application route handlers, domain integration clients, and user interface components requiring shared utility routines.

### Rules

- **R-UTIL-001** SHOULD NOT: Consuming modules SHOULD NOT create localized duplicate utility functions when equivalent functionality exists within lib/utils.

### Verify

```bash
# Discover and execute the project static analysis and type verification script
# Discover and execute the project test runner
```

**Accept when:**
- All consuming modules resolve imports from lib/utils with zero compilation or static analysis diagnostics.
- All automated unit tests covering functions in lib/utils pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks and peer code review.
</enforcement>