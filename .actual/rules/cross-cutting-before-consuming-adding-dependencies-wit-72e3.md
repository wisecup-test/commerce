---
version: 2
name: cross-cutting-before-consuming-adding-dependencies-wit-72e3
description: "Adoption of lib/utils Internal Shared Module: Before Consuming Adding Dependencies Within Shared"
metadata:
  actual:
    adr_id: 72e30286-45b2-482c-a5fe-6cd10e4672ed
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

# Adoption of lib/utils Internal Shared Module: Before Consuming Adding Dependencies Within Shared

Scope: applies to `app/**`, `components/layout/search/filter/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for application route handlers, domain integration clients, and user interface components requiring shared utility routines.

### Rules

- **R-LIB-001** MUST: Before consuming or adding dependencies within the shared utility module, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency versions.

### Verify

```bash
# Discover and execute the project static analysis and type verification script
# Discover and execute the project test runner
```

**Accept when:**
- All consuming modules resolve imports from lib/utils with zero compilation or static analysis diagnostics.
- All automated unit tests covering functions in lib/utils pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks in continuous integration pipelines and peer code review.
</enforcement>