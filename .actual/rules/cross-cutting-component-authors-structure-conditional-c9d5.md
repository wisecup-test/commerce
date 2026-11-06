---
version: 2
name: cross-cutting-component-authors-structure-conditional-c9d5
description: "Adoption of clsx for Conditional Class Name Composition: Component Authors Structure Conditional Classes Object"
metadata:
  actual:
    adr_id: c9d5d4ef-6853-4a38-8e5c-8dba1568d9fb
    status: accepted
    candidate_group_id: e269d76a-28d5-498f-8cd2-9136c635a668
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of clsx for Conditional Class Name Composition: Component Authors Structure Conditional Classes Object

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for user interface components and presentation modules requiring conditional or dynamic class names.

### Rules

- **R-CLS-001** SHOULD: Component authors structure conditional classes using object mapping syntax or boolean expressions supported by the utility function.
- **R-CLS-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the repository.
- **R-CLS-003** MANDATORY: Before writing code that uses a versioned library, execute lock-version grounding in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use.
- **R-CLS-004** MANDATORY: Import the utility function consistently across component modules to handle dynamic class evaluation, avoiding ad-hoc helper implementations.
- **R-CLS-005** MANDATORY: Pass class names as distinct arguments or object entries where keys represent CSS class names and values represent boolean conditions.

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
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline failure occurs when raw string concatenation or disallowed class joining patterns are detected.
</enforcement>