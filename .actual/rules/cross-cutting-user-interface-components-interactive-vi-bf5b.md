---
version: 2
name: cross-cutting-user-interface-components-interactive-vi-bf5b
description: "React Core Library Adoption: User Interface Components Interactive Views Standardize"
metadata:
  actual:
    adr_id: bf5bfc73-0670-49ad-a9ea-50f4b6ffae3b
    status: accepted
    candidate_group_id: dd4cf757-029b-4c55-a073-09368a803835
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
      - components/**
---

# React Core Library Adoption: User Interface Components Interactive Views Standardize

Scope: applies to `app/search/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: User interface components and interactive views MUST standardize on React as the core declarative library for component definitions and state management.
- **R-REACT-002** MANDATORY: Prior to writing code that uses a versioned library, execute the lock-version grounding process: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact to determine the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use.
- **R-REACT-003** MANDATORY: Structure interactive state management using contextual providers and dedicated custom hooks to decouple data promises from presentation layouts.
- **R-REACT-004** MANDATORY: Organize component hierarchies such that interactive leaves declare client runtime directives while parent layout wrappers remain pure rendering structures.

### Verify

```bash
# Discover and execute the repository type-checking script to validate React component signatures
# Discover and execute the repository static analysis and linting scripts
# Discover and execute the repository test suite
```

**Accept when:**
- All component definitions type-check successfully with zero errors across interface modules.
- Static analysis validates that all hook calls conform to dependency constraints without unescaped imperative view mutations.
- Automated test suites pass without regressions across interactive views and state context providers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All component definitions, hook dependencies, and rendering directives must be verified via static analysis, type checking, and test suites.
</enforcement>