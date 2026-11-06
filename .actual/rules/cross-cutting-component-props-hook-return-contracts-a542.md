---
version: 2
name: cross-cutting-component-props-hook-return-contracts-a542
description: "Adoption of lib/shopify/types Module for Centralized Domain Types: Component Props Hook Return Contracts Representing"
metadata:
  actual:
    adr_id: a542ffbb-af39-4fd0-8bfe-b16e761078b5
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

# Adoption of lib/shopify/types Module for Centralized Domain Types: Component Props Hook Return Contracts Representing

Scope: applies to `components/cart/**`, `components/grid/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for frontend presentation components, layout structures, and client state contexts interacting with commerce domain entities, including product catalogs, product variant selections, and shopping cart operations.

### Rules

- **R-SHOPIFY-001** SHOULD: Component props and hook return contracts representing domain entities such as product variants or cart line items SHOULD explicitly reference typed interfaces exported by the lib/shopify/types module rather than unconstrained object signatures.

### Verify

```bash
# Discover and run the project static typecheck command
# Discover and run the project linter command
```

**Accept when:**
- The project typecheck process completes with zero type errors across all modules importing lib/shopify/types.
- All components consuming commerce domain structures explicitly reference types from the central domain module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static type checking, linting, and peer code review.
</enforcement>