---
version: 2
name: cross-cutting-modules-not-introduce-duplicate-competin-6884
description: "Adoption of clsx for Conditional Class Name Composition: Modules Not Introduce Duplicate Competing Class"
metadata:
  actual:
    adr_id: 6884eaf8-a678-4abd-bd18-a1eed252f66f
    status: accepted
    candidate_group_id: e269d76a-28d5-498f-8cd2-9136c635a668
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of clsx for Conditional Class Name Composition: Modules Not Introduce Duplicate Competing Class

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CLS-001** MUST_NOT: Modules MUST NOT introduce duplicate or competing class name joining libraries into the dependency graph.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>