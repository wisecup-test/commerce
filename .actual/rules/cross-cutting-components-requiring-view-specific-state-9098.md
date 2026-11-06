---
version: 2
name: cross-cutting-components-requiring-view-specific-state-9098
description: "Adoption of lib/shopify/types Module for Centralized Domain Types: Components Requiring View Specific State Compose"
metadata:
  actual:
    adr_id: 90989843-330b-4c54-9240-569c555948fe
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

# Adoption of lib/shopify/types Module for Centralized Domain Types: Components Requiring View Specific State Compose

Scope: applies to `components/cart/**`, `components/grid/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all frontend presentation components, layout structures, and client state contexts interacting with commerce domain entities.

### Rules

- **R-SHOP-001** MAY: Components requiring view-specific UI state MAY compose or extend base domain types from the lib/shopify/types module provided that underlying domain entity contracts remain unmodified.

### Verify

```bash
# Discover and run the project static typecheck command to verify all component implementations conform to the centralized domain type contracts
npm run typecheck

# Discover and run the project linter command to ensure no prohibited local duplications of domain types exist
npm run lint
```

**Accept when:**
- The project typecheck process completes with zero type errors across all modules importing lib/shopify/types.
- All components consuming commerce domain structures explicitly reference types from the central domain module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>