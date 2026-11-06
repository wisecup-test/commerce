---
version: 2
name: cross-cutting-components-requiring-commerce-domain-dat-5d1c
description: "Adoption of lib/shopify as Core E-Commerce Integration Module: Components Requiring Commerce Domain Data Operations"
metadata:
  actual:
    adr_id: 5d1c25af-0a0e-4d93-af53-5511e7e9a2b1
    status: accepted
    candidate_group_id: 4351d544-a887-4734-b4b3-6c3c9ad9ed15
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/**
---

# Adoption of lib/shopify as Core E-Commerce Integration Module: Components Requiring Commerce Domain Data Operations

Scope: applies to `components/layout/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: Components requiring e-commerce domain data or operations MUST route all domain requests through the lib/shopify module rather than implementing direct transport calls or alternative data layers.

### Verify

```bash
# Discover and execute the repository type checking script
# Discover and run the architectural linter configured in the project
```

**Accept when:**
- Static type analysis passes with zero type errors across all layout component imports.
- Boundary linting confirms all e-commerce operations originate exclusively from lib/shopify.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration checks, peer code reviews, and architectural linting rules.
</enforcement>