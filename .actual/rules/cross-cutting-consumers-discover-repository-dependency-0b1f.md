---
version: 2
name: cross-cutting-consumers-discover-repository-dependency-0b1f
description: "Adoption of lib/shopify as Core E-Commerce Integration Module: Consumers Discover Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 0b1fa0e1-08a0-4a31-b606-2aae940fa9c9
    status: accepted
    candidate_group_id: 4351d544-a887-4734-b4b3-6c3c9ad9ed15
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/**
---

# Adoption of lib/shopify as Core E-Commerce Integration Module: Consumers Discover Repository Dependency Manifest Lock

Scope: applies to `components/layout/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOP-001** MUST: Consumers MUST discover the repository dependency manifest and lock artifact to resolve and verify exact locked versions of all runtime dependencies supporting the module prior to implementation.

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