---
version: 2
name: cross-cutting-presentational-variations-such-dimension-34e1
description: "Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Presentational Variations Such Dimensions Color Classes"
metadata:
  actual:
    adr_id: 34e1d5d2-d35c-46bc-add2-e23c06df585d
    status: accepted
    candidate_group_id: 3b7fb158-9916-4e65-972c-4185a31a0e1f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Presentational Variations Such Dimensions Color Classes

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for:
- Rendering corporate or brand logo assets in client-side or server-rendered user interface components.
- Generating dynamic visual metadata artifacts, including social media preview images.

### Rules

- **R-LM-001** SHOULD: Presentational variations such as dimensions, color classes, or container styling SHOULD be passed as structured properties to the ./icons/logo component rather than hardcoded within consuming components.

### Verify

- Discover the project verification script from the repository manifest and execute component testing across all visual modules importing ./icons/logo.
- Execute the repository visual regression and linting suites discovered in the project automation pipeline to validate brand rendering integrity.

**Accept when:**

- All user interface components and metadata generators render brand assets exclusively through the shared ./icons/logo module with zero inline SVG duplicates.
- Component test suites and visual regression checks pass without layout shifts or rendering failures across both interactive and metadata generation contexts.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and lint rules forbidding duplicated brand SVG paths in application components.
- Verified by: Peer code reviews and architectural checks on pull requests modifying presentation and metadata layers.
- Verified by: Continuous integration test passes verifying component rendering and visual regression outputs.
- Violation handling: Automated continuous integration checks reject pull requests containing direct SVG duplicates of the brand logo.
- Violation handling: Engineers must refactor violating components to consume the shared ./icons/logo module before merge approval.
- Exception process: Submit an architectural review request citing the specific rendering constraint that precludes shared module import.
- Exception process: Obtain explicit approval from the frontend architecture lead with documented fallback rationale.
</enforcement>
