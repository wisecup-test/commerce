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

These rules are ALWAYS ACTIVE for:
- User interface components and presentation modules requiring conditional or dynamic class names.

### Rules

- **R-LM-001** SHOULD: Component authors SHOULD structure conditional classes using object mapping syntax or boolean expressions supported by the utility function.

### Verify

- sh -c 'echo "Discover and execute repository linting script to validate component class composition"'
- sh -c 'echo "Discover and execute repository build verification script to ensure dependency lock compliance"'

**Accept when:**

- The project static analysis and linting suite completes with zero violations across all UI components.
- All dynamic class name evaluations in component markup use clsx rather than manual string concatenation.
- The repository dependency resolution artifact confirms the locked utility version is resolved and active.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and lint checks executed during continuous integration.
- Verified by: Peer code review for all pull requests modifying UI components.
- Violation handling: Continuous integration pipeline failure when raw string concatenation or disallowed class joining patterns are detected.
- Violation handling: Mandatory refactoring of non-compliant class concatenation prior to merge approval.
- Exception process: Exceptions require formal architecture review and explicit documentation outlining the technical constraints that prevent utility adoption.
</enforcement>
