---
version: 2
name: cross-cutting-before-implementing-updating-components-a7df
description: "React Core Library Adoption: Before Implementing Updating Components React Consumer"
metadata:
  actual:
    adr_id: a7dffb3d-e138-4621-81b0-9a811b9df18d
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

# React Core Library Adoption: Before Implementing Updating Components React Consumer

Scope: applies to `app/search/**`, `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: Before implementing or updating components using React, the consumer MUST inspect the repository dependency manifest and lock artifact to identify and verify the exact resolved version against authoritative version documentation.

### Verify

```bash
# Discover and execute the repository type-checking script to validate React component signatures, hook parameter types, and context definitions
# Discover and execute the repository static analysis and linting scripts to verify compliance with rendering directives and hook dependency guidelines
# Discover and execute the repository test suite to confirm that reactive components and context providers render and transition state as expected
```

**Accept when:**
- All component definitions type-check successfully with zero errors across interface modules.
- Static analysis validates that all hook calls conform to dependency constraints without unescaped imperative view mutations.
- Automated test suites pass without regressions across interactive views and state context providers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced via automated static analysis, type verification, and peer code review.
</enforcement>