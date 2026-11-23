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

These rules are ALWAYS ACTIVE for:
- Frontend presentation components, layout structures, and client state contexts interacting with commerce domain entities.
- Components handling product catalogs, product variant selections, and shopping cart operations.

### Rules

- **R-LM-001** MUST: Developers MUST discover the project manifest and resolve exact dependency versions from the repository lock artifact prior to introducing or updating external library integrations that interface with domain types.

### Verify

- Discover and run the project static typecheck command to verify all component implementations conform to the centralized domain type contracts without type diagnostics.
- Discover and run the project linter command to ensure no prohibited local duplications of domain types or unconstrained object usage exist.

**Accept when:**

- The project typecheck process completes with zero type errors across all modules importing lib/shopify/types.
- All components consuming commerce domain structures explicitly reference types from the central domain module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static type checking and linting executed during continuous integration workflows.
- Verified by: Peer code review verifying that new commerce-facing components utilize the shared domain type module.
- Violation handling: Pull requests introducing local duplicate domain types or failing typecheck verification are blocked from merging until resolved.
- Exception process: Exceptions must be submitted via an architecture review request detailing the necessity for distinct contracts and specifying a migration plan.
</enforcement>
