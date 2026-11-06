---
version: 2
name: cross-cutting-cart-query-mutation-definitions-returnin-dc31
description: "Shopify Cart Fragment Module Adoption: Cart Query Mutation Definitions Returning Entity"
metadata:
  actual:
    adr_id: dc310ddc-8bf0-4f35-9436-9199b95bf247
    status: accepted
    candidate_group_id: 7bc01f66-13c9-48f1-bd0f-3b89df4574ac
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Cart Fragment Module Adoption: Cart Query Mutation Definitions Returning Entity

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all cart query and mutation definitions returning cart entity payloads.

### Rules

- **R-CART-001** MUST: All cart query and mutation definitions returning cart entity payloads MUST compose their response selections using the shared cart fragment module.

### Verify

```bash
# Discover and execute the project's static analysis, query validation, and type-checking suites
# (Commands to be discovered from the repository manifest and build tool)
```

**Accept when:**
- All cart query and mutation definitions successfully compile and incorporate the shared cart fragment module without validation errors.
- Static type checking passes across all cart query and mutation modules with zero schema divergence.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing cart queries or mutations with ad-hoc field selections that bypass the shared cart fragment module are blocked from merging.
</enforcement>