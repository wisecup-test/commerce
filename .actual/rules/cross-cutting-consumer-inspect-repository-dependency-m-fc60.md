---
version: 2
name: cross-cutting-consumer-inspect-repository-dependency-m-fc60
description: "Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Consumer Inspect Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: fc603d22-4c1d-4906-b533-a1ff0508dab7
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

# Adopt @headlessui/react for Accessible Unstyled UI Primitives in Overlay Components: Consumer Inspect Repository Dependency Manifest Lock

Scope: applies to `components/cart/**`, `components/layout/navbar/**`.

These rules are ALWAYS ACTIVE for:
- Interactive client-side overlay components requiring accessible state, focus management, and transition coordination.

### Rules

- **R-LM-001** MUST: The consumer MUST inspect the repository dependency manifest and lock resolution artifact to discover and bind to the exact resolved version of @headlessui/react before implementing or upgrading overlay components.

### Verify

- Discover and execute the project static analysis scripts to verify that interactive overlay modules import headless primitive components from @headlessui/react.
- Discover and execute the project test runner to validate keyboard navigation, focus trapping, and modal dismissal behavior across overlay components.

**Accept when:**

- Static analysis verifies that interactive modal, drawer, and overlay components import unstyled headless primitives from @headlessui/react rather than custom focus traps.
- Component test suites confirm correct focus entrapment, keyboard escape dismissal, and accessible role assignment across all interactive overlay dialogs.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and linting rules inspecting imports within client-rendered overlay modules.
- Verified by: Peer code review verifying accessibility properties and focus coordination during interface updates.
- Violation handling: Pull requests introducing bespoke focus traps or ad-hoc modal overlays without headless primitives are blocked pending remediation.
- Violation handling: Violations must replace ad-hoc overlay state management with approved headless primitive components.
- Exception process: Submit an architectural variance request detailing why headless primitives cannot meet the interaction or platform requirements.
- Exception process: Obtain approval from the frontend engineering leads and document the exception in the component specification.
</enforcement>
