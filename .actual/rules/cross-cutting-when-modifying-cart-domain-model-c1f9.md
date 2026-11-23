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

These rules are ALWAYS ACTIVE for:
- Data retrieval queries that return cart domain entity representations
- State-modifying mutations that return updated cart domain entity representations

### Rules

- **R-LM-001** MUST: When modifying the cart domain model, changes to cart field selections MUST be introduced directly within the shared cart fragment module rather than in individual query or mutation definitions.

### Verify

- Discover and execute the project's static analysis and query validation suite to ensure all cart query and mutation documents compile with the shared cart fragment.
- Discover and run the project's type-checking suite to confirm that generated query and mutation response types resolve against the shared cart fragment definition.

**Accept when:**

- All cart query and mutation definitions successfully compile and incorporate the shared cart fragment module without validation errors.
- Static type checking passes across all cart query and mutation modules with zero schema divergence.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and schema validation suites executed in continuous integration pipelines
- Verified by: Peer code reviews for all pull requests modifying cart query or mutation modules
- Violation handling: Pull requests containing cart queries or mutations with ad-hoc field selections that bypass the shared cart fragment module are blocked from merging.
- Exception process: Exceptions require architectural review documenting specific payload budget constraints that necessitate an isolated query definition.
</enforcement>
