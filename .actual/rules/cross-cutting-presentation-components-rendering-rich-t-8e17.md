---
version: 2
name: cross-cutting-presentation-components-rendering-rich-t-8e17
description: "Adoption of components/prose Module for Rich Text Typography: Presentation Components Rendering Rich Text Article"
metadata:
  actual:
    adr_id: 8e1752d7-d3f9-4303-91cb-79e1e31462f5
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

# Adoption of components/prose Module for Rich Text Typography: Presentation Components Rendering Rich Text Article

Scope: applies to `app/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROSE-001** MUST: UI presentation components rendering rich text, article content, or dynamic descriptive copy MUST delegate content display to the components/prose internal module rather than authoring bespoke typographic utility classes.

### Verify

```bash
# Discover and execute project test runner script to verify component rendering integrity
# Discover and execute repository linter script to verify compliance with module import conventions
# Discover and execute project build verification command to ensure static type consistency
```

**Accept when:**
- The project test suite passes with all rich text rendering components mounting the prose module without errors.
- The project linter passes with zero violations regarding prohibited inline typography formatting in presentation templates.
- The project build verification command completes successfully with no type resolution or import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>