---
version: 2
name: cross-cutting-presentation-layout-components-not-invok-f27c
description: "Adoption of lib/shopify as Core E-Commerce Integration Module: Presentation Layout Components Not Invoke Third"
metadata:
  actual:
    adr_id: f27cf41e-d6f5-4da7-bb87-43729770478b
    status: accepted
    candidate_group_id: 4351d544-a887-4734-b4b3-6c3c9ad9ed15
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/**
---

# Adoption of lib/shopify as Core E-Commerce Integration Module: Presentation Layout Components Not Invoke Third

Scope: applies to `components/layout/**`.

These rules are ALWAYS ACTIVE for user interface components, structural layout elements, and data-fetching operations interacting with the external e-commerce platform.

### Rules

- **R-SHOPIFY-001** MUST_NOT: Presentation layout components MUST NOT invoke third-party e-commerce endpoints directly or bypass the lib/shopify module boundary.

### Verify

```bash
# Discover and execute the repository type checking script to verify layout components consume lib/shopify
# Discover and run the architectural linter to verify no direct external e-commerce API calls exist outside of lib/shopify
```

**Accept when:**
- Static type analysis passes with zero type errors across all layout component imports.
- Boundary linting confirms all e-commerce operations originate exclusively from lib/shopify.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>