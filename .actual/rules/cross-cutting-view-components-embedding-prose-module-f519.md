---
version: 2
name: cross-cutting-view-components-embedding-prose-module-f519
description: "Adoption of components/prose Module for Rich Text Typography: View Components Embedding Prose Module Supply"
metadata:
  actual:
    adr_id: f519567e-0ff2-45bc-bb22-5962221e43cc
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

# Adoption of components/prose Module for Rich Text Typography: View Components Embedding Prose Module Supply

Scope: applies to `app/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROSE-001** SHOULD: View components embedding the prose module SHOULD supply layout boundaries and outer spacing through parent container classes while leaving typographic scale and text styling to the prose component.

### Verify

```bash
# Discover the project test runner script from the package manifest and execute the test suite
# Discover the repository linter script from the build configuration and execute it
# Discover the project build verification command from the workspace manifest and execute a production build
```

**Accept when:**
- The project test suite passes with all rich text rendering components mounting the prose module without errors.
- The project linter passes with zero violations regarding prohibited inline typography formatting in presentation templates.
- The project build verification command completes successfully with no type resolution or import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, peer code reviews, and automated linters will block pull requests or reject builds that bypass standardized presentation components or contain inline typography overrides on rich text containers.
</enforcement>