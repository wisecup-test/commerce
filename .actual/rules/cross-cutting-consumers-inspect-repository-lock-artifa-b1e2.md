---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-b1e2
description: "Adoption of clsx for Conditional Class Name Composition: Consumers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: b1e27640-0295-4fd7-ae90-cacd57fcaacb
    status: accepted
    candidate_group_id: e269d76a-28d5-498f-8cd2-9136c635a668
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of clsx for Conditional Class Name Composition: Consumers Inspect Repository Lock Artifact Verify

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for user interface components and presentation modules requiring conditional or dynamic class names.

### Rules

- **R-CLS-001** MUST: Consumers MUST inspect the repository lock artifact and verify the exact resolved version of the utility before writing or updating component styling logic.

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
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis and peer review; violations fail CI.
</enforcement>