---
version: 2
name: cross-cutting-consumers-querying-subordinate-domain-en-a1fa
description: "lib/shopify In-Memory Data Access: Consumers Querying Subordinate Domain Entities Lib"
metadata:
  actual:
    adr_id: a1fabe5f-ec5a-4bf5-807d-ae795e45998e
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

# lib/shopify In-Memory Data Access: Consumers Querying Subordinate Domain Entities Lib

Scope: applies to `app/search/**`, `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: Consumers querying subordinate domain entities from lib/shopify collections MUST perform in-memory predicate matching against retrieved collection arrays rather than issuing secondary remote network queries.

### Verify

```bash
# Discover and execute test suite covering domain data access functions
# Discover and execute static analysis and type checking verification scripts
```

**Accept when:**
- All discovered test suites covering domain data access functions execute and pass with zero failures.
- Discovered static analysis and type verification routines report zero errors across all modules accessing domain collections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated static analysis, type checking, and peer code review is mandatory.
</enforcement>