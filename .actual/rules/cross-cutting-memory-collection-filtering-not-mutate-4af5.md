---
version: 2
name: cross-cutting-memory-collection-filtering-not-mutate-4af5
description: "lib/shopify In-Memory Data Access: Memory Collection Filtering Not Mutate Underlying"
metadata:
  actual:
    adr_id: 4af52142-c0f3-46dc-864c-475a636a342d
    status: accepted
    candidate_group_id: 0c82ddb0-1946-4982-a199-cb6a1a72d36a
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
      - components/cart/**
      - components/product/**
---

# lib/shopify In-Memory Data Access: Memory Collection Filtering Not Mutate Underlying

Scope: applies to `app/search/**`, `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for client and server components accessing nested items within composite domain models provided by lib/shopify.

### Rules

- **R-SHOULD-001** SHOULD_NOT: In-memory collection filtering MUST NOT mutate the underlying collection arrays retrieved from lib/shopify.

### Verify

```bash
# Discover and run test suite covering domain data access functions
# Discover and run static analysis and type checking verification scripts
```

**Accept when:**
- All discovered test suites covering domain data access functions execute and pass with zero failures.
- Discovered static analysis and type verification routines report zero errors across all modules accessing domain collections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and type checking within continuous integration pipelines, as well as peer code review, verify correct predicate matching and null-handling on collection searches.
</enforcement>