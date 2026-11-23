---
version: 2
name: cross-cutting-functions-exported-lib-utils-remain-e842
description: "Adoption of lib/utils Internal Shared Module: Functions Exported Lib Utils Remain Pure"
metadata:
  actual:
    adr_id: e842bf34-83e8-4d51-a760-80c4002b5a0d
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

# Adoption of lib/utils Internal Shared Module: Functions Exported Lib Utils Remain Pure

Scope: applies to `app/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for:
- Application route handlers, domain integration clients, and user interface components requiring shared utility routines.

### Rules

- **R-LM-001** SHOULD: Functions exported from lib/utils SHOULD remain pure, side-effect free, and fully typed to ensure safe execution in both server and client execution contexts.

### Verify

- Discover and execute the project static analysis and type verification script to ensure all references to lib/utils satisfy type contracts.
- Discover and execute the project test runner to validate that utility functions pass all unit tests.

**Accept when:**

- All consuming modules resolve imports from lib/utils with zero compilation or static analysis diagnostics.
- All automated unit tests covering functions in lib/utils pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis checks in continuous integration pipelines
- Verified by: Peer code review during pull request evaluation
- Violation handling: Pull requests introducing duplicate utility logic or circular dependencies are blocked until resolved.
- Exception process: Exceptions require documented approval from the technical architecture lead detailing why localized utilities are strictly necessary.
</enforcement>
