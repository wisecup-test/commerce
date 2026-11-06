---
version: 2
name: cross-cutting-page-templates-view-components-not-dba1
description: "Adoption of components/prose Module for Rich Text Typography: Page Templates View Components Not Apply"
metadata:
  actual:
    adr_id: dba1a9ba-6a66-4ab7-91a1-433b83e54f4b
    status: accepted
    candidate_group_id: ef1ad579-dd4f-481b-8561-23724c669a3d
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
      - components/product/**
---

# Adoption of components/prose Module for Rich Text Typography: Page Templates View Components Not Apply

Scope: applies to `app/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROSE-001** MUST_NOT: Page templates and view components MUST_NOT apply direct typography overrides to internal HTML elements rendered within the prose container hierarchy.

### Verify

```bash
# Discover and execute the project test runner script from the package manifest
# Discover and execute the repository linter script from the build configuration
# Discover and execute the project build verification command from the workspace manifest
```

**Accept when:**
- The project test suite passes with all rich text rendering components mounting the prose module without errors.
- The project linter passes with zero violations regarding prohibited inline typography formatting in presentation templates.
- The project build verification command completes successfully with no type resolution or import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>