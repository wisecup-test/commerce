---
version: 2
name: cross-cutting-components-requiring-complex-variant-lin-f38f
description: "lib/shopify In-Memory Data Access: Components Requiring Complex Variant Line Item"
metadata:
  actual:
    adr_id: f38f8981-f410-417b-b634-2dd2454d0987
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

# lib/shopify In-Memory Data Access: Components Requiring Complex Variant Line Item

Scope: applies to `app/search/**`, `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for client and server components accessing nested items within composite domain models provided by lib/shopify.

### Rules

- **R-SHOPIFY-001** MAY: Components requiring complex variant or line item resolution MAY encapsulate matching predicates within reusable helper functions or hooks.

### Verify

```bash
# Discover and execute project test runner
# Discover and execute static analysis and type checking verification scripts
```

**Accept when:**
- All discovered test suites covering domain data access functions execute and pass with zero failures.
- Discovered static analysis and type verification routines report zero errors across all modules accessing domain collections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis and type checking within continuous integration pipelines will block merge requests until resolved.
</enforcement>