---
version: 2
name: cross-cutting-components-requiring-iconography-import-b318
description: "Adoption of @heroicons/react/24/outline for User Interface Iconography: Components Requiring Iconography Import Icon Representations"
metadata:
  actual:
    adr_id: b3186f3e-00b9-402f-b3e9-b40571be633b
    status: accepted
    candidate_group_id: 79132c3d-c9da-4982-ba2a-7e35341d6aea
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - components/layout/**
      - components/product/**
---

# Adoption of @heroicons/react/24/outline for User Interface Iconography: Components Requiring Iconography Import Icon Representations

Scope: applies to `components/cart/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all user interface components displaying decorative or action-associated iconography in navigation, search, cart, and product views.

### Rules

- **R-ICON-001** MUST: Components requiring iconography MUST import icon representations exclusively from the @heroicons/react/24/outline module path.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute it
# to validate that all icon imports resolve against the locked dependency version.
# Also execute repository linting and static analysis tasks.
npm run lint || yarn lint || pnpm lint || bun run lint
```

**Accept when:**
- All interactive user interface controls render iconography sourced exclusively from @heroicons/react/24/outline without static analysis errors.
- The repository verification suite passes with zero unresolved module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. All icon imports must be validated via static analysis and code review checks against approved module paths.
</enforcement>