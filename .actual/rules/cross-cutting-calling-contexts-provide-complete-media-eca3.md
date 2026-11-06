---
version: 2
name: cross-cutting-calling-contexts-provide-complete-media-eca3
description: "Adoption of components/grid/tile Shared Presentation Module: Calling Contexts Provide Complete Media Properties"
metadata:
  actual:
    adr_id: eca34fb5-f211-498f-ae4e-f32b07d1eb1d
    status: accepted
    candidate_group_id: f5522651-25ba-4ef5-8b2d-69cfef172d1b
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/product/**
      - components/grid/**
      - components/layout/**
      - components/product/**
---

# Adoption of components/grid/tile Shared Presentation Module: Calling Contexts Provide Complete Media Properties

Scope: applies to `app/product/**`, `components/grid/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GRID-001** SHOULD: Calling contexts SHOULD provide complete media properties and navigation descriptors through the standardized interface contract to maintain uniform aspect ratios and image handling.

### Verify

```bash
echo "Discover the project dependency manifest and lock artifact to verify resolved environment configurations."
echo "Execute the repository static analysis and linting scripts to verify compliance with shared component boundaries."
echo "Execute the repository test suite to confirm visual tile integration and regression test passes across all dependent surfaces."
```

**Accept when:**
- All catalog and showcase presentations import and render the shared grid tile module without bespoke styling overrides
- Repository verification and static analysis pipelines pass without architecture boundary violations

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>