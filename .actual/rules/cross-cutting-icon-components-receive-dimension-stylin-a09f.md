---
version: 2
name: cross-cutting-icon-components-receive-dimension-stylin-a09f
description: "Adoption of @heroicons/react/24/outline for User Interface Iconography: Icon Components Receive Dimension Styling Constraints"
metadata:
  actual:
    adr_id: a09ff6cf-8637-481b-a368-3ca6c12b6ea5
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

# Adoption of @heroicons/react/24/outline for User Interface Iconography: Icon Components Receive Dimension Styling Constraints

Scope: applies to `components/cart/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HERO-001** SHOULD: Icon components SHOULD receive dimension and styling constraints through standard style property classes rather than hardcoded vector attributes.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute it to validate that all icon imports resolve against the locked dependency version.
# Execute the repository linting and static analysis tasks to ensure no unauthorized icon libraries or unapproved inline vector graphics have been introduced.
```

**Accept when:**
- All interactive user interface controls render iconography sourced exclusively from @heroicons/react/24/outline without static analysis errors.
- The repository verification suite passes with zero unresolved module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. All interactive user interface controls must render iconography sourced exclusively from @heroicons/react/24/outline, and code containing unauthorized icon imports or raw inline SVG icons without an approved exception will be blocked.
</enforcement>