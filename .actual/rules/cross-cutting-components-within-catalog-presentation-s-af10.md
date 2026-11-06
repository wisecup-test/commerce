---
version: 2
name: cross-cutting-components-within-catalog-presentation-s-af10
description: "Adoption of components/grid Shared Module for Product and Loading Layouts: Components Within Catalog Presentation Scope Not"
metadata:
  actual:
    adr_id: af10c2f6-59bd-4198-b7b2-13c7352aa815
    status: accepted
    candidate_group_id: 310194c3-206f-4538-8e18-43ec81b266d7
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
      - components/layout/**
---

# Adoption of components/grid Shared Module for Product and Loading Layouts: Components Within Catalog Presentation Scope Not

Scope: applies to `app/search/**`, `components/layout/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CAT-001** MUST_NOT: Components within the catalog presentation scope MUST NOT define bespoke top-level grid containers that bypass the shared components/grid module.

### Verify

```bash
# Discover and run the project static analysis and lint script from the repository manifest.
# Discover and execute the project test runner script to validate layout component rendering.
```

**Accept when:**
- All catalog and loading routes import and render the shared grid module without unresolved dependencies.
- All static analysis and component test suites complete successfully without violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>