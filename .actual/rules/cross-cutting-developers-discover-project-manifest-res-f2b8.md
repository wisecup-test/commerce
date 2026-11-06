---
version: 2
name: cross-cutting-developers-discover-project-manifest-res-f2b8
description: "Adoption of lib/shopify/types Module for Centralized Domain Types: Developers Discover Project Manifest Resolve Exact"
metadata:
  actual:
    adr_id: f2b8f1d5-af7e-438c-962b-a5f553b82c3a
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

# Adoption of lib/shopify/types Module for Centralized Domain Types: Developers Discover Project Manifest Resolve Exact

Scope: applies to `components/cart/**`, `components/grid/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for frontend presentation components, layout structures, and client state contexts interacting with commerce domain entities, handling product catalogs, product variant selections, and shopping cart operations.

### Rules

- **R-ADR-001** MUST: Developers MUST discover the project manifest and resolve exact dependency versions from the repository lock artifact prior to introducing or updating external library integrations that interface with domain types.
- **R-ADR-002** MUST: Discover the project centralized domain type definitions by inspecting the shared domain module before declaring new types for commerce entities.
- **R-ADR-003** MUST: Ensure all newly introduced UI components interacting with catalog or cart state import domain entities directly from the central module rather than recreating local interfaces.

### Verify

```bash
# Discover and run the project static typecheck command
# Discover and run the project linter command
```

**Accept when:**
- The project typecheck process completes with zero type errors across all modules importing lib/shopify/types.
- All components consuming commerce domain structures explicitly reference types from the central domain module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static type checking and linting executed during continuous integration workflows and peer code review.
</enforcement>