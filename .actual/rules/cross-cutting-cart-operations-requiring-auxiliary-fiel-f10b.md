---
version: 2
name: cross-cutting-cart-operations-requiring-auxiliary-fiel-f10b
description: "Shopify Cart Fragment Module Adoption: Cart Operations Requiring Auxiliary Fields Not"
metadata:
  actual:
    adr_id: f10bdac8-cdab-4a2c-b808-3eddf99e19d9
    status: accepted
    candidate_group_id: 7bc01f66-13c9-48f1-bd0f-3b89df4574ac
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Cart Fragment Module Adoption: Cart Operations Requiring Auxiliary Fields Not

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all cart query and mutation modules in the project.

### Rules

- **R-CART-001** SHOULD: Cart operations requiring auxiliary fields not present in the base cart entity SHOULD compose specialized field selections alongside the shared cart fragment rather than replacing it.

### Verify

```bash
# Discover and execute the project's static analysis and query validation suite
# Discover and run the project's type-checking suite
```

**Accept when:**
- All cart query and mutation definitions successfully compile and incorporate the shared cart fragment module without validation errors.
- Static type checking passes across all cart query and mutation modules with zero schema divergence.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>