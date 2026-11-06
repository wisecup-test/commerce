---
version: 2
name: cross-cutting-components-consuming-commerce-data-impor-d865
description: "Adoption of lib/shopify as Core E-Commerce Integration Module: Components Consuming Commerce Data Import Shared"
metadata:
  actual:
    adr_id: d865e829-fa6b-4a83-b527-d386ba74aa6e
    status: accepted
    candidate_group_id: 4351d544-a887-4734-b4b3-6c3c9ad9ed15
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/**
---

# Adoption of lib/shopify as Core E-Commerce Integration Module: Components Consuming Commerce Data Import Shared

Scope: applies to `components/layout/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOP-001** MUST: Components consuming e-commerce data MUST import shared entity contracts from lib/shopify/types rather than defining local duplicate type definitions.

### Verify

```bash
# Discover and execute the repository type checking script to verify layout components consume lib/shopify and lib/shopify/types without compilation errors
npm run typecheck

# Discover and run the architectural linter to verify no direct external e-commerce API calls exist outside of lib/shopify
npm run lint:architecture
```

**Accept when:**
- Static type analysis passes with zero type errors across all layout component imports.
- Boundary linting confirms all e-commerce operations originate exclusively from lib/shopify.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>