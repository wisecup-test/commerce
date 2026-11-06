---
version: 2
name: cross-cutting-presentation-components-not-use-raw-dab6
description: "Adoption of next/image for UI Image Presentation: Presentation Components Not Use Raw Unmanaged"
metadata:
  actual:
    adr_id: dab63da9-3959-4fbd-8f20-de1d556b6391
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

# Adoption of next/image for UI Image Presentation: Presentation Components Not Use Raw Unmanaged

Scope: applies to `components/grid/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for frontend user interface components rendering static or dynamic raster images, including product gallery and grid tile presentation components.

### Rules

- **R-IMG-001** MUST_NOT: Presentation components MUST NOT use raw unmanaged HTML image tags for raster image display.

### Verify

```bash
# Discover and execute project test commands to validate component rendering test suites
# Discover and run project static analysis scripts to verify imports and image component usage
```

**Accept when:**
- Visual display components render image assets exclusively through the next/image module.
- Project automated static analysis and test validation suites pass without unmanaged image tag warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code review enforce that presentation components do not use raw unmanaged image tags.
</enforcement>