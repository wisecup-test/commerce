---
version: 2
name: cross-cutting-item-components-specify-responsive-span-0b83
description: "Adoption of components/grid Shared Module for Product and Loading Layouts: Item Components Specify Responsive Span Sizing"
metadata:
  actual:
    adr_id: 0b836caa-b349-43b3-bb45-3531c5b24b06
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

# Adoption of components/grid Shared Module for Product and Loading Layouts: Item Components Specify Responsive Span Sizing

Scope: applies to `app/search/**`, `components/layout/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GRID-001** MAY: Item components MAY specify responsive span or sizing overrides supported by the grid interface when rendering specialized promotional or highlighted catalog entries.

### Verify

```bash
# Discover and run the project static analysis and lint script from the repository manifest.
# Discover and execute the project test runner script to validate layout component rendering.
```

**Accept when:**
- All catalog and loading routes import and render the shared grid module without unresolved dependencies.
- All static analysis and component test suites complete successfully without violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>