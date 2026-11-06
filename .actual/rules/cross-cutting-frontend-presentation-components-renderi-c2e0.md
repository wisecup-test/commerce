---
version: 2
name: cross-cutting-frontend-presentation-components-renderi-c2e0
description: "Adoption of next/image for UI Image Presentation: Frontend Presentation Components Rendering Image Assets"
metadata:
  actual:
    adr_id: c2e01228-d5c1-4a14-af5b-70dd72f492fe
    status: accepted
    candidate_group_id: e06a20a8-3618-46f3-9226-1b35388989a5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/grid/**
      - components/product/**
---

# Adoption of next/image for UI Image Presentation: Frontend Presentation Components Rendering Image Assets

Scope: applies to `components/grid/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for frontend presentation components rendering image assets.

### Rules

- **R-IMG-001** MUST: Frontend presentation components rendering image assets MUST use the next/image module for image presentation.

### Verify

```bash
# Discover and run project static analysis scripts to verify imports and image component usage
# Discover and execute project test commands to validate component rendering test suites
```

**Accept when:**
- Visual display components render image assets exclusively through the next/image module.
- Project automated static analysis and test validation suites pass without unmanaged image tag warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>