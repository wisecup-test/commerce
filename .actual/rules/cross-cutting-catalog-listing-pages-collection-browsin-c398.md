---
version: 2
name: cross-cutting-catalog-listing-pages-collection-browsin-c398
description: "Adoption of components/grid Shared Module for Product and Loading Layouts: Catalog Listing Pages Collection Browsing Views"
metadata:
  actual:
    adr_id: c3983885-f888-4edc-be76-5321fd2a6fcc
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

# Adoption of components/grid Shared Module for Product and Loading Layouts: Catalog Listing Pages Collection Browsing Views

Scope: applies to `app/search/**`, `components/layout/**`.

These rules are ALWAYS ACTIVE for all search result pages, collection browsing pages, product display components, and associated loading boundaries rendering multi-item visual layouts.

### Rules

- **R-GRID-001** MUST: Catalog listing pages, collection browsing views, and associated loading states MUST utilize the components/grid shared module to define multi-item responsive grid structures rather than implementing custom ad-hoc layout wrappers.

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