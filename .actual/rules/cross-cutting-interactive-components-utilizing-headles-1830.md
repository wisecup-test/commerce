---
version: 2
name: cross-cutting-interactive-components-utilizing-headles-1830
description: "Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Interactive Components Utilizing Headlessui React Primitives"
metadata:
  actual:
    adr_id: 1830be6b-414a-4d62-aa52-2762e7190238
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

# Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Interactive Components Utilizing Headlessui React Primitives

Scope: applies to `components/cart/**`, `components/layout/navbar/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HEAD-001** MUST: Interactive components utilizing @headlessui/react primitives MUST declare explicit client-side component execution boundaries.

### Verify

```bash
# Discover and execute the project static analysis scripts to verify that interactive overlay modules import headless primitive components from @headlessui/react.
# Discover and execute the project test runner to validate keyboard navigation, focus trapping, and modal dismissal behavior across overlay components.
```

**Accept when:**
- Static analysis verifies that interactive modal, drawer, and overlay components import unstyled headless primitives from @headlessui/react rather than custom focus traps.
- Component test suites confirm correct focus entrapment, keyboard escape dismissal, and accessible role assignment across all interactive overlay dialogs.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>