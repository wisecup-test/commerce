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

These rules are ALWAYS ACTIVE for:
- Data retrieval queries that return cart domain entity representations
- State-modifying mutations that return updated cart domain entity representations

### Rules

- **R-LM-001** MUST: All cart query and mutation definitions returning cart entity payloads MUST compose their response selections using the shared cart fragment module.

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
