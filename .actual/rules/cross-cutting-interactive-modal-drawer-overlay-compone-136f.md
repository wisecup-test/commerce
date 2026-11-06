---
version: 2
name: cross-cutting-interactive-modal-drawer-overlay-compone-136f
description: "Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Interactive Modal Drawer Overlay Components Adopt"
metadata:
  actual:
    adr_id: 136f7426-e51d-4015-abb5-2e7a6352c61d
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

# Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Interactive Modal Drawer Overlay Components Adopt

Scope: applies to `components/cart/**`, `components/layout/navbar/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HEAD-001** MUST: Interactive modal, drawer, and overlay components MUST adopt @headlessui/react primitives to coordinate accessibility semantics, focus containment, and disclosure state.

### Verify

```bash
# Discover and execute project static analysis and test runner scripts as defined in the repository manifest.
```

**Accept when:**
- Static analysis verifies that interactive modal, drawer, and overlay components import unstyled headless primitives from @headlessui/react rather than custom focus traps.
- Component test suites confirm correct focus entrapment, keyboard escape dismissal, and accessible role assignment across all interactive overlay dialogs.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>