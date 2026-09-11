# Adopt React Hooks for Client-Side Interactivity: Components Requiring Client Side Interactivity Declared

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The project requires dynamic and interactive user interfaces.
- A consistent approach to managing client-side component state and side effects is necessary.
- The chosen framework provides mechanisms for declarative UI development.
- The need to enable rich user experiences directly in the browser.

## Problem Statement

How to manage client-side state and side effects in interactive UI components effectively and consistently across the application.

## Decision

1. MUST: Components requiring client-side interactivity MUST be declared with the `"use client"` directive.

## Policy Block

- MUST Components requiring client-side interactivity MUST be declared with the `"use client"` directive.

In scope:
- Components requiring client-side state management or side effects.
- Interactive UI elements such as dropdowns, modals, navigation menus, and toasts.

Out of scope:
- Server-side components.
- Purely static UI elements without interactivity.
- Components where state management is handled exclusively by a global store.

## Rationale

- React hooks provide a powerful and declarative way to manage state and side effects in functional components.
- The use of `useState` and `useEffect` promotes component reusability and testability.
- The `"use client"` directive explicitly marks components for client-side rendering, ensuring interactivity.
- This approach aligns with modern React development practices.

## Consequences

Positive:
- Improved component reusability and maintainability.
- More predictable state management and side effect handling.
- Clearer separation of concerns within components.
- Enhanced developer experience through functional component patterns.

Negative:
- Potential for performance issues if `useEffect` dependency arrays are not managed carefully.
- Increased bundle size for client-side components compared to purely server-rendered ones.
- Steeper learning curve for developers unfamiliar with React hooks.

## Alternatives

- Using React Class Components (rejected)
  Rejected because: Modern React development favors hooks for cleaner, more functional components with better reusability and less boilerplate.
  When valid: For maintaining legacy codebases that extensively use class components.
- Using an external state management library for local component state (rejected)
  Rejected because: For local component state and simple side effects, built-in React hooks are sufficient and reduce bundle size and complexity.
  When valid: For complex, global state management needs that span multiple, deeply nested components.

## Risks

- Performance degradation due to incorrect or missing `useEffect` dependency arrays, leading to unnecessary re-renders or infinite loops.
  Mitigation: Implement strict linting rules for React hooks, conduct thorough code reviews, and utilize React DevTools for performance profiling.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Prioritize the use of functional components over class components for new interactive UI features.
- Ensure proper cleanup functions are returned from `useEffect` hooks to prevent memory leaks and unexpected behavior.

## Continuation Context


Verify commands:
- Discover and run the project's linting commands.
- Discover and run the project's test suite for UI components.
- Discover and run the project's build commands to ensure successful compilation.

Accept when:
- Linting passes without warnings or errors related to React hooks usage.
- All relevant unit and integration tests for interactive components pass.
- The application builds successfully, and interactive components function as expected in a browser environment.

## Enforcement

- Verified by: CI/CD pipelines with automated linting and testing.
- Verified by: Mandatory code reviews for all UI component changes.
- Violation handling: Automated build failures for linting or test violations.
- Violation handling: Mandatory code review comments requiring adherence to hook usage guidelines.
- Exception process: Exceptions to this ADR require a documented architectural review and approval process, outlining the specific justification and alternative approach.