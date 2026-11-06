---
version: 2
name: cross-cutting-overlay-components-consuming-headlessui-840f
description: "Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Overlay Components Consuming Headlessui React Isolate"
metadata:
  actual:
    adr_id: 840f2912-963c-4d0e-9495-c6cd94945ec2
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

# Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Overlay Components Consuming Headlessui React Isolate

Scope: applies to `components/cart/**`, `components/layout/navbar/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OVERLAY-001** MUST: Overlay components consuming @headlessui/react MUST isolate visual presentation to consumer-level utility styling and MUST NOT suppress or overwrite internal accessible role and state attributes.

### Verify

```bash
# Discover and execute the project static analysis scripts to verify that interactive overlay modules import headless primitive components from @headlessui/react.
# Discover and execute the project test runner to validate keyboard navigation, focus trapping, and modal dismissal behavior across overlay components.
```

**Accept when:**
- Static analysis verifies that interactive modal, drawer, and overlay components import unstyled headless primitives from @headlessui/react rather than custom focus traps.
- Component test suites confirm correct focus entrapment, keyboard escape dismissal, and accessible role assignment across all interactive overlay dialogs.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, linting, and peer code review enforce that ad-hoc modal overlays and bespoke focus traps are blocked pending remediation.
</enforcement>