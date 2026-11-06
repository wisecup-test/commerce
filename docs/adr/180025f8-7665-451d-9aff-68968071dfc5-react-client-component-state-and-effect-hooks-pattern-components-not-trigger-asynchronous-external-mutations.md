---
version: 2
name: 180025f8-7665-451d-9aff-68968071dfc5-react-client-component-state-and-effect-hooks-pattern-components-not-trigger-asynchronous-external-mutations
description: "React Client Component State and Effect Hooks Pattern: Components Not Trigger Asynchronous External Mutations"
metadata:
  actual:
    adr_id: 180025f8-7665-451d-9aff-68968071dfc5
    status: accepted
    candidate_group_id: 47fe226a-46d7-4ea3-8c60-35676ab361f8
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# React Client Component State and Effect Hooks Pattern: Components Not Trigger Asynchronous External Mutations

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/**`

## Context

- UI interaction components require localized state management, browser event handlers, and synchronization with client-side navigation or external UI state.
- The rendering architecture distinguishes between server components and client boundary modules requiring explicit client directives.
- Client components across navigation, cart operations, modal dialogues, and toast notifications consistently declare client execution boundaries and consume interaction hooks.

## Problem Statement

User interface elements requiring interactive state, DOM event listeners, and client-side lifecycle effects cannot execute within server component environments without explicit boundary declaration and consistent local state orchestration, leading to runtime failures if interactive hooks are improperly placed across component boundaries.

## Decision

1. SHOULD_NOT: Components SHOULD NOT trigger asynchronous external mutations directly inside useEffect when an explicit user event handler can encapsulate the action.

## Policy Block

- SHOULD_NOT Components SHOULD NOT trigger asynchronous external mutations directly inside useEffect when an explicit user event handler can encapsulate the action.

In scope:
- Interactive UI components handling user input, modal visibility, menu state, and navigation filter interactions.
- Client context providers coordinating shared client-side application state.

Out of scope:
- Static presentational components that render markup without client-side state or event listeners.
- Server-rendered layout structures and data-fetching boundaries that do not attach interactive handlers.

## Rationale

- Enforcing explicit client directive boundaries ensures interactive React hooks operate exclusively within client-hydrated environments, preventing server execution errors.
- Consolidating interaction state into standard React hooks provides predictable component lifecycle handling across navigation, cart, modal, and toast interactions without introducing external state managers for local concerns.
- Memoizing derived state and synchronizing navigation parameters via standard hooks maintains reactive UI state while minimizing redundant render cycles.

## Consequences

Positive:
- Clear separation of server rendering boundaries from interactive client hydration islands.
- Predictable component lifecycle and state encapsulation using standard framework primitives.
- Improved client-side rendering performance through selective memoization and localized re-renders.

Negative:
- Increased client bundle payload size for components designated with the client boundary directive.
- Potential hydration mismatch risks if initial client state differs from server-rendered markup during effect execution.
- Discipline required to prevent overusing effect hooks for logic that belongs in event handlers.

## Alternatives

- Global state management store for all interaction states (rejected)
  Rejected because: Introducing an external global state store for localized UI interactions like dropdown toggles and modal visibility adds architectural complexity and unnecessary bundle weight.
  When valid: When interaction states must be shared globally across deeply nested, disparate component trees that cannot be served by contextual providers.
- Direct DOM manipulation and vanilla script event binding (rejected)
  Rejected because: Direct DOM manipulation bypasses the declarative UI reconciliation cycle, causing state desynchronization and breaking hydration guarantees.
  When valid: When embedding isolated third-party non-declarative widgets that manage their own self-contained lifecycle.

## Risks

- Excessive or cascading re-renders caused by unmemoized values or poorly guarded useEffect dependencies.
  Mitigation: Enforce static analysis rules for hook dependency arrays and memoize derived calculations using useMemo.
  Owner: engineering team
- Hydration mismatch errors caused by client-only state altering DOM structure immediately upon mounting.
  Mitigation: Defer client-dependent rendering until after hydration completes or align initial server state with default client state.
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
- Ensure that every component consuming useState, useEffect, or useMemo includes the client directive at the top of the file before imports or code.
- Structure client boundary components at leaf levels of the component hierarchy to keep the client JavaScript bundle minimal.

## Continuation Context


Verify commands:
- Discover the test script in the project manifest and execute the test runner to verify interactive component behavior.
- Discover the linting script in the project manifest and execute the static analyzer to check client directive and hook usage.
- Discover the build verification script in the project manifest and execute the compiler to confirm type safety and client boundary exports.

Accept when:
- Static analysis validates that all components consuming interactive hooks declare the client component directive.
- Unit and component tests verify that state transitions and effect hooks function correctly across user interactions.
- The build compilation step completes without type errors or server-client boundary violations.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline validating hook usage and directive presence.
- Verified by: Peer code reviews verifying appropriate separation of client interaction boundaries and state management.
- Violation handling: Pull requests containing interactive hooks in non-client components fail automated continuous integration checks.
- Violation handling: Code review comments mandate refactoring when interactive state logic violates component boundary guidelines.
- Exception process: Architectural exceptions require written documentation and approval from the frontend technical lead.