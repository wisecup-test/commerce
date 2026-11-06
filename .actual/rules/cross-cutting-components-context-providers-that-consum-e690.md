---
version: 2
name: cross-cutting-components-context-providers-that-consum-e690
description: "Adoption of lib/shopify/types Module for Centralized Domain Types: Components Context Providers That Consume Transform"
metadata:
  actual:
    adr_id: e69006d8-8af4-47b8-96a5-89174cbc7167
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

# Adoption of lib/shopify/types Module for Centralized Domain Types: Components Context Providers That Consume Transform

Scope: applies to `components/cart/**`, `components/grid/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for frontend presentation components, layout structures, and client state contexts interacting with commerce domain entities (catalog, product variant selections, cart operations).

### Rules

- **R-SHOP-001** MUST: All components and context providers that consume, transform, or render commerce domain entities MUST import and adhere to structural type definitions declared in the lib/shopify/types module.

### Verify

```bash
# Discover and run the project static typecheck command
# Discover and run the project linter command
```

**Accept when:**
- The project typecheck process completes with zero type errors across all modules importing lib/shopify/types.
- All components consuming commerce domain structures explicitly reference types from the central domain module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static type checking, linting, and peer code review are enforced during CI workflows.
</enforcement>