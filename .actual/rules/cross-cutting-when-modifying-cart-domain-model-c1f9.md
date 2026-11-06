---
version: 2
name: cross-cutting-when-modifying-cart-domain-model-c1f9
description: "Shopify Cart Fragment Module Adoption: When Modifying Cart Domain Model Changes"
metadata:
  actual:
    adr_id: c1f94573-b1eb-4275-8c79-4d39e04f66e4
    status: accepted
    candidate_group_id: 7bc01f66-13c9-48f1-bd0f-3b89df4574ac
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Cart Fragment Module Adoption: When Modifying Cart Domain Model Changes

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CART-001** MUST: When modifying the cart domain model, changes to cart field selections MUST be introduced directly within the shared cart fragment module rather than in individual query or mutation definitions.

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