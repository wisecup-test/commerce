---
version: 2
name: cross-cutting-memory-collection-searches-validate-enti-f885
description: "lib/shopify In-Memory Data Access: Memory Collection Searches Validate Entity Presence"
metadata:
  actual:
    adr_id: f8852df2-c483-4f07-986e-4d2f3632b096
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

# lib/shopify In-Memory Data Access: Memory Collection Searches Validate Entity Presence

Scope: applies to `app/search/**`, `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: In-memory collection searches MUST validate entity presence and handle null or undefined results when matching identifiers or composite option criteria.

### Verify

```bash
# Discover and run test runner and static analysis/type checking scripts from project configuration
```

**Accept when:**
- All discovered test suites covering domain data access functions execute and pass with zero failures.
- Discovered static analysis and type verification routines report zero errors across all modules accessing domain collections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>