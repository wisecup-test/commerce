---
version: 2
name: cross-cutting-new-domain-operations-extending-store-6203
description: "Adoption of lib/shopify as Core E-Commerce Integration Module: New Domain Operations Extending Store Capabilities"
metadata:
  actual:
    adr_id: 6203fdab-74fb-4e25-9239-dcb11269d15a
    status: accepted
    candidate_group_id: 4351d544-a887-4734-b4b3-6c3c9ad9ed15
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/**
---

# Adoption of lib/shopify as Core E-Commerce Integration Module: New Domain Operations Extending Store Capabilities

Scope: applies to `components/layout/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOULD-001** SHOULD: New domain operations extending store capabilities be implemented as exported functions within lib/shopify to preserve a single integration surface.

### Verify

```bash
# Discover and execute the repository type checking script and architectural linter as configured in the project repository.
```

**Accept when:**
- Static type analysis passes with zero type errors across all layout component imports.
- Boundary linting confirms all e-commerce operations originate exclusively from lib/shopify.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute type verification and import boundary rules.
</enforcement>