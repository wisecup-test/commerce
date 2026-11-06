---
version: 2
name: cross-cutting-components-requiring-mocked-data-during-f2be
description: "Adoption of lib/shopify as Core E-Commerce Integration Module: Components Requiring Mocked Data During Isolated"
metadata:
  actual:
    adr_id: f2be54f6-5464-4272-a24b-283186791701
    status: accepted
    candidate_group_id: 4351d544-a887-4734-b4b3-6c3c9ad9ed15
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/**
---

# Adoption of lib/shopify as Core E-Commerce Integration Module: Components Requiring Mocked Data During Isolated

Scope: applies to `components/layout/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MAY: Components requiring mocked data during isolated verification MAY substitute the lib/shopify module exports with compliant test doubles.

### Verify

```bash
# Discover and execute the repository type checking script
# Discover and run the architectural linter configured in the project
```

**Accept when:**
- Static type analysis passes with zero type errors across all layout component imports.
- Boundary linting confirms all e-commerce operations originate exclusively from lib/shopify.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>