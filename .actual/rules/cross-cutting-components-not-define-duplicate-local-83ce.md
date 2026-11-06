---
version: 2
name: cross-cutting-components-not-define-duplicate-local-83ce
description: "Adoption of lib/shopify/types Module for Centralized Domain Types: Components Not Define Duplicate Local Variations"
metadata:
  actual:
    adr_id: 83cec1b8-2e86-4e5a-aa2e-dbaf39d6820c
    status: accepted
    candidate_group_id: e4172a9d-5b98-40c4-9425-311efc04f707
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - components/grid/**
      - components/layout/**
      - components/product/**
---

# Adoption of lib/shopify/types Module for Centralized Domain Types: Components Not Define Duplicate Local Variations

Scope: applies to `components/cart/**`, `components/grid/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOP-001** MUST_NOT: Components MUST NOT define duplicate or local variations of domain interfaces when an equivalent entity type is provided by the lib/shopify/types module.

### Verify

```bash
# Discover and run the project static typecheck command
# Discover and run the project linter command
```

**Accept when:**
- The project typecheck process completes with zero type errors across all modules importing lib/shopify/types.
- All components consuming commerce domain structures explicitly reference types from the central domain module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>