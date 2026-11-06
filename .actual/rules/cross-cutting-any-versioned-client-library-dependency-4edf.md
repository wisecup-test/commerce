---
version: 2
name: cross-cutting-any-versioned-client-library-dependency-4edf
description: "Shopify Cart Fragment Module Adoption: Any Versioned Client Library Dependency Referenced"
metadata:
  actual:
    adr_id: 4edf655c-0fa7-41c5-920f-12359c348e36
    status: accepted
    candidate_group_id: 7bc01f66-13c9-48f1-bd0f-3b89df4574ac
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Cart Fragment Module Adoption: Any Versioned Client Library Dependency Referenced

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CART-001** MUST: If any versioned client library or dependency is referenced by the fragment or operations, the consumer MUST inspect the ecosystem lock artifact to verify the exact resolved dependency version before implementation.
- **R-CART-002** MUST: Compose all newly defined cart query and mutation operations by importing and interpolating the shared cart fragment module definition.
- **R-CART-003** MUST: Verify that document interpolation syntax adheres to the schema validation requirements of the project data client.

### Verify

```bash
# Discover and execute the project's static analysis and query validation suite
# Discover and run the project's type-checking suite
# (Commands must be discovered from the repository structure)
```

**Accept when:**
- All cart query and mutation definitions successfully compile and incorporate the shared cart fragment module without validation errors.
- Static type checking passes across all cart query and mutation modules with zero schema divergence.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cart queries and mutations must be verified against static analysis and type-checking suites.
</enforcement>