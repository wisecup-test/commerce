---
version: 2
name: cross-cutting-user-interface-component-layer-use-e444
description: "Adoption of clsx for Conditional Class Name Composition: User Interface Component Layer Use Clsx"
metadata:
  actual:
    adr_id: e4447f0c-b163-4a48-af62-ebfa5489458b
    status: accepted
    candidate_group_id: e269d76a-28d5-498f-8cd2-9136c635a668
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of clsx for Conditional Class Name Composition: User Interface Component Layer Use Clsx

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CLS-001** MUST: The user interface component layer MUST use clsx as the primary utility function for resolving conditional and dynamic class names.
- **R-CLS-002** MUST: Follow lock-version grounding before writing code that uses a versioned library: discover manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm APIs exist, and re-run for version-sensitive behavior.
- **R-CLS-003** MUST: Import the utility function consistently across component modules to handle dynamic class evaluation, avoiding ad-hoc helper implementations.
- **R-CLS-004** MUST: Pass class names as distinct arguments or object entries where keys represent CSS class names and values represent boolean conditions.

### Verify

```bash
sh -c 'echo "Discover and execute repository linting script to validate component class composition"'
sh -c 'echo "Discover and execute repository build verification script to ensure dependency lock compliance"'
```

**Accept when:**
- The project static analysis and linting suite completes with zero violations across all UI components.
- All dynamic class name evaluations in component markup use clsx rather than manual string concatenation.
- The repository dependency resolution artifact confirms the locked utility version is resolved and active.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying UI components are verified via automated static analysis, lint checks, and peer code review.
</enforcement>