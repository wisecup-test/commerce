---
version: 2
name: cross-cutting-cart-queries-mutations-not-declare-712d
description: "Shopify Cart Fragment Module Adoption: Cart Queries Mutations Not Declare Hoc"
metadata:
  actual:
    adr_id: 712d6c1d-6dd4-4658-9537-0bbcdd49ed80
    status: accepted
    candidate_group_id: 7bc01f66-13c9-48f1-bd0f-3b89df4574ac
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Cart Fragment Module Adoption: Cart Queries Mutations Not Declare Hoc

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CART-001** MUST_NOT: Cart queries and mutations MUST NOT declare ad-hoc or inline cart entity field selections that duplicate or diverge from the shared cart fragment module.

### Verify

```bash
# Discover and execute the project's static analysis and query validation suite
# Discover and run the project's type-checking suite
# (Repository-specific build tools and verification commands must be derived from the project manifest)
```

**Accept when:**
- All cart query and mutation definitions successfully compile and incorporate the shared cart fragment module without validation errors.
- Static type checking passes across all cart query and mutation modules with zero schema divergence.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis, schema validation suites, and peer code reviews.
</enforcement>