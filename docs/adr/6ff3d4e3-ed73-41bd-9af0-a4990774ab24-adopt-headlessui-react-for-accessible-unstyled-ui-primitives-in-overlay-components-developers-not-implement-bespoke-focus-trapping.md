---
version: 2
name: 6ff3d4e3-ed73-41bd-9af0-a4990774ab24-adopt-headlessui-react-for-accessible-unstyled-ui-primitives-in-overlay-components-developers-not-implement-bespoke-focus-trapping
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

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/cart/**`
- `components/layout/navbar/**`

## Context

- Interactive overlay components such as navigation drawers and cart dialogs require accessible focus trapping, keyboard navigation, and disclosure state coordination.
- Implementing accessible overlay behaviors from scratch introduces recurring accessibility regressions and inconsistent user interaction states.
- The codebase adopts utility-driven styling and requires unstyled component primitives that do not enforce rigid design tokens.

## Problem Statement

Building interactive UI overlays such as modals and navigation drawers requires complex accessibility mechanics, including focus entrapment, escape key handling, and ARIA attribute synchronisation, which become inconsistent and error-prone when implemented manually across separate components.

## Decision

1. SHOULD_NOT: Developers SHOULD NOT implement bespoke focus-trapping routines or manual ARIA modal states when corresponding @headlessui/react primitive components satisfy the interaction requirement.

## Policy Block

- SHOULD_NOT Developers SHOULD NOT implement bespoke focus-trapping routines or manual ARIA modal states when corresponding @headlessui/react primitive components satisfy the interaction requirement.

In scope:
- Interactive client-side overlay components requiring accessible state, focus management, and transition coordination.

Out of scope:
- Static non-interactive UI elements and server-rendered components without overlay or interactive state requirements.

## Rationale

- The codebase consistently imports @headlessui/react across distinct overlay components, demonstrating a deliberate standard for accessible interactive disclosures.
- Headless primitives provide full accessibility compliance while giving consumers complete ownership over CSS utility styling.
- Using tested interactive primitives mitigates accessibility regressions without locking the application into a heavy visual component library.

## Consequences

Positive:
- Guarantees robust keyboard navigation, focus management, and accessibility tree compliance across overlay surfaces.
- Decouples accessible interaction logic from visual presentation, enabling unrestricted consumer styling.
- Standardizes modal and drawer behavior across independent interface modules through shared headless primitives.

Negative:
- Increases client runtime bundle size by introducing external module dependencies for interactive UI primitives.
- Requires ongoing alignment with the external library's API conventions and lifecycle updates across major releases.

## Alternatives

- Bespoke modal state and manual ARIA focus trap implementation (rejected)
  Rejected because: Manual implementation of focus containment, keyboard listeners, and screen reader announcements is defect-prone and increases maintenance burden across interactive components.
  When valid: Environments with strict zero-dependency runtime constraints where external module imports are impermissible.
- Opinionated pre-styled component libraries with bundled visual design tokens (rejected)
  Rejected because: Pre-styled component frameworks impose opinionated styling abstractions that conflict with custom utility class styling and increase bundle weight with unneeded styling overhead.
  When valid: Rapid prototyping initiatives using off-the-shelf design systems without bespoke interface styling requirements.

## Risks

- Mismatched library versions across workspace modules leading to divergent overlay state behaviors.
  Mitigation: Verify the exact resolved dependency version from repository lock artifacts before invoking component APIs.
  Owner: engineering team
- Client-side execution overhead from interactive primitives impacting low-bandwidth environments.
  Mitigation: Confine headless interactive primitives to client component boundaries and defer rendering of overlays until activated.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure overlay wrappers isolate transition and backdrop rendering to dedicated disclosure sub-components provided by the headless library.
- Apply utility styling directly to primitive elements while preserving default keyboard handlers and focus restoration behaviors.

## Continuation Context


Verify commands:
- Discover and execute the project static analysis scripts to verify that interactive overlay modules import headless primitive components from @headlessui/react.
- Discover and execute the project test runner to validate keyboard navigation, focus trapping, and modal dismissal behavior across overlay components.

Accept when:
- Static analysis verifies that interactive modal, drawer, and overlay components import unstyled headless primitives from @headlessui/react rather than custom focus traps.
- Component test suites confirm correct focus entrapment, keyboard escape dismissal, and accessible role assignment across all interactive overlay dialogs.

## Enforcement

- Verified by: Automated static analysis and linting rules inspecting imports within client-rendered overlay modules.
- Verified by: Peer code review verifying accessibility properties and focus coordination during interface updates.
- Violation handling: Pull requests introducing bespoke focus traps or ad-hoc modal overlays without headless primitives are blocked pending remediation.
- Violation handling: Violations must replace ad-hoc overlay state management with approved headless primitive components.
- Exception process: Submit an architectural variance request detailing why headless primitives cannot meet the interaction or platform requirements.
- Exception process: Obtain approval from the frontend engineering leads and document the exception in the component specification.