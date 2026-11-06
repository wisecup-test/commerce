---
version: 2
name: cross-cutting-individual-grid-items-within-catalog-981a
description: "Adoption of components/grid Shared Module for Product and Loading Layouts: Individual Grid Items Within Catalog Views"
metadata:
  actual:
    adr_id: 981a213d-41a8-47fc-b9da-287bf4c6b072
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

# Adoption of components/grid Shared Module for Product and Loading Layouts: Individual Grid Items Within Catalog Views

Scope: applies to `app/search/**`, `components/layout/**`.

These rules are ALWAYS ACTIVE for all search result pages, collection browsing pages, product display components, and associated loading boundaries rendering multi-item visual layouts.

### Rules

- **R-GRID-001** SHOULD: Individual grid items within catalog views SHOULD be composed through dedicated tile layout components rather than direct container nesting.

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