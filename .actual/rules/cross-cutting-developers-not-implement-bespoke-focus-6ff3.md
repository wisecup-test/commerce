---
version: 2
name: cross-cutting-developers-not-implement-bespoke-focus-6ff3
description: "Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Developers Not Implement Bespoke Focus Trapping"
metadata:
  actual:
    adr_id: 6ff3d4e3-ed73-41bd-9af0-a4990774ab24
    status: accepted
    candidate_group_id: 22a75313-e58c-43ae-bd1d-3019111b934a
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - components/layout/navbar/**
---

# Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Developers Not Implement Bespoke Focus Trapping

Scope: applies to `components/cart/**`, `components/layout/navbar/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HEADLESS-001** SHOULD_NOT: Developers SHOULD NOT implement bespoke focus-trapping routines or manual ARIA modal states when corresponding @headlessui/react primitive components satisfy the interaction requirement.

### Verify

```bash
# Discover and execute project static analysis scripts to verify interactive overlay modules import headless primitive components
# Discover and execute project test runner to validate keyboard navigation, focus trapping, and modal dismissal behavior
```

**Accept when:**
- Static analysis verifies that interactive modal, drawer, and overlay components import unstyled headless primitives from @headlessui/react rather than custom focus traps.
- Component test suites confirm correct focus entrapment, keyboard escape dismissal, and accessible role assignment across all interactive overlay dialogs.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code review enforce that pull requests introducing bespoke focus traps or ad-hoc modal overlays without headless primitives are blocked pending remediation.
</enforcement>