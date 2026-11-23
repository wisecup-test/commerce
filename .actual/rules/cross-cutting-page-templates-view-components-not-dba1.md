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

These rules are ALWAYS ACTIVE for:
- Rendering dynamic multi-paragraph HTML or markdown text content in route templates and view components.
- Displaying product descriptions and long-form editorial copy requiring standardized typography.

### Rules

- **R-LM-001** MUST_NOT: Page templates and view components MUST_NOT apply direct typography overrides to internal HTML elements rendered within the prose container hierarchy.

### Verify

- Discover the project test runner script from the package manifest and execute the test suite to verify component rendering integrity.
- Discover the repository linter script from the build configuration and execute it to verify compliance with module import conventions.
- Discover the project build verification command from the workspace manifest and execute a production build to ensure static type consistency.

**Accept when:**

- The project test suite passes with all rich text rendering components mounting the prose module without errors.
- The project linter passes with zero violations regarding prohibited inline typography formatting in presentation templates.
- The project build verification command completes successfully with no type resolution or import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Continuous integration pipelines executing repository linter and test validation scripts.
- Verified by: Peer code review verifying that rich text display delegates to the prose module.
- Violation handling: Pull requests containing inline typography overrides on rich text containers will be blocked until refactored to use the prose module.
- Violation handling: Automated lint failures will reject builds that bypass standardized presentation components.
- Exception process: Exceptions require documented justification detailing why standard prose typography cannot satisfy the view constraints, along with engineering team approval.
</enforcement>
