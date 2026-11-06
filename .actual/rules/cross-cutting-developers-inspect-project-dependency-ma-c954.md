---
version: 2
name: cross-cutting-developers-inspect-project-dependency-ma-c954
description: "React Client Component State and Effect Hooks Pattern: Developers Inspect Project Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: c9548e0f-02f4-4607-ba67-4618849146b1
    status: accepted
    candidate_group_id: 47fe226a-46d7-4ea3-8c60-35676ab361f8
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# React Client Component State and Effect Hooks Pattern: Developers Inspect Project Dependency Manifest Authoritative

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: Developers MUST inspect the project dependency manifest and authoritative lock artifact to verify the exact resolved version of React and its supported hook capabilities before implementation.
- **R-REACT-002** MUST: Ensure that every component consuming useState, useEffect, or useMemo includes the client directive at the top of the file before imports or code.
- **R-REACT-003** SHOULD: Structure client boundary components at leaf levels of the component hierarchy to keep the client JavaScript bundle minimal.

### Verify

```bash
# Discover and execute the test script from the project manifest
# Discover and execute the linting script from the project manifest
# Discover and execute the build verification script from the project manifest
```

**Accept when:**
- Static analysis validates that all components consuming interactive hooks declare the client component directive.
- Unit and component tests verify that state transitions and effect hooks function correctly across user interactions.
- The build compilation step completes without type errors or server-client boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and build compilation must pass without server-client boundary violations.
</enforcement>