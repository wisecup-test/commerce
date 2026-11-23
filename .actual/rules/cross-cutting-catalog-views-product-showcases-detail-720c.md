---
version: 2
name: cross-cutting-catalog-views-product-showcases-detail-720c
description: "Adoption of components/grid/tile Shared Presentation Module: Catalog Views Product Showcases Detail Layouts"
metadata:
  actual:
    adr_id: 720c6a92-01b1-4528-a508-188ad1d4afe7
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

# Adoption of components/grid/tile Shared Presentation Module: Catalog Views Product Showcases Detail Layouts

Scope: applies to `app/product/**`, `components/grid/**`, `components/layout/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for:
- User-facing interfaces that present catalog items, showcase arrangements, image groupings, or product grids

### Rules

- **R-LM-001** MUST: All catalog views, product showcases, and detail layouts MUST standardize on the shared components/grid/tile module for rendering individual visual item display units rather than constructing bespoke tile structures.

### Verify

- echo "Discover the project dependency manifest and lock artifact to verify resolved environment configurations."
- echo "Execute the repository static analysis and linting scripts to verify compliance with shared component boundaries."
- echo "Execute the repository test suite to confirm visual tile integration and regression test passes across all dependent surfaces."

**Accept when:**

- All catalog and showcase presentations import and render the shared grid tile module without bespoke styling overrides
- Repository verification and static analysis pipelines pass without architecture boundary violations

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis verifying import paths and module boundary constraints in integration pipelines
- Verified by: Peer code review validating that catalog views delegate tile rendering to the shared module
- Violation handling: Integration pipeline failures on unauthorized bespoke tile implementations
- Violation handling: Code review rejection of PRs that re-implement tile presentation logic rather than consuming the shared module
- Exception process: Submit an architectural review request explaining why the shared tile interface cannot satisfy the required presentation semantics
- Exception process: Obtain formal approval from the UI architecture maintainers before introducing any alternate tile implementation
</enforcement>
