---
version: 2
name: cross-cutting-before-adopting-modifying-dependencies-c-a538
description: "Adoption of components/prose Module for Rich Text Typography: Before Adopting Modifying Dependencies Consumed Prose"
metadata:
  actual:
    adr_id: a53840f9-7dac-429a-8d0e-f2fe92c3e697
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

# Adoption of components/prose Module for Rich Text Typography: Before Adopting Modifying Dependencies Consumed Prose

Scope: applies to `app/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROSE-001** MUST: Before adopting or modifying dependencies consumed by the prose module, developers MUST inspect the repository package manifest and resolution lock artifact to verify the exact resolved versions of styling and rendering libraries.

### Verify

```bash
# Discover and execute project test runner script
# Discover and execute repository linter script
# Discover and execute project build verification command
```

**Accept when:**
- The project test suite passes with all rich text rendering components mounting the prose module without errors.
- The project linter passes with zero violations regarding prohibited inline typography formatting in presentation templates.
- The project build verification command completes successfully with no type resolution or import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, peer code reviews, and automated lint rules will enforce prose module usage and block violations.
</enforcement>