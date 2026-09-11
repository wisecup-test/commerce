# Adoption of `next/navigation` and `react` for Client-Side Interactive Components: Declare Client Side Components Use Directive

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The project utilizes a component-based architecture for building user interfaces.
- Interactive UI elements require client-side state management and lifecycle handling.
- URL query parameters are used to manage and reflect UI state, such as sorting or filtering.
- A need exists for a consistent approach to client-side rendering and navigation interactions.

## Problem Statement

The project requires a standardized approach for developing interactive client-side UI components that effectively manage their internal state and synchronize with URL query parameters for persistent and shareable application state.

## Decision

1. MUST: Declare client-side components with the `"use client"` directive when they require interactive features or hooks that are not supported in server components.

## Policy Block

- MUST Declare client-side components with the `"use client"` directive when they require interactive features or hooks that are not supported in server components.

In scope:
- Client-side interactive UI components.
- Components that read or write to URL query parameters.
- Components requiring `useState` or `useEffect`.

Out of scope:
- Server-side components that do not require client-side interactivity.
- Purely presentational components without state or side effects.

## Rationale

- Adopting `react` provides a robust framework for building declarative and reactive user interfaces, enabling efficient state management and component reusability.
- The integration of `next/navigation` allows for seamless interaction with URL parameters, which is crucial for maintaining application state across page loads and enabling deep linking.
- The `"use client"` directive explicitly defines the rendering environment, ensuring that interactive components function correctly within the client-side context.
- Standardizing on these libraries promotes consistency and reduces cognitive load for developers working on interactive UI features.

## Consequences

Positive:
- Consistent approach to client-side state management and URL interaction.
- Improved developer productivity due to established patterns and widely adopted libraries.
- Enhanced user experience through interactive and stateful UI components.
- Clear separation of client and server concerns with the `"use client"` directive.

Negative:
- Increased bundle size for client-side components compared to purely server-rendered ones.
- Potential for client-side hydration issues if not carefully managed.
- Dependency on specific versions of `react` and `next/navigation`.

## Alternatives

- Use a different frontend framework (e.g., Vue, Angular). (rejected)
  Rejected because: The project already has established patterns and components built with `react` and `next/navigation`, switching would incur significant refactoring costs.
  When valid: For new projects without existing frontend infrastructure.
- Manage URL state manually without `next/navigation`. (rejected)
  Rejected because: Manual URL state management is error-prone, less efficient, and lacks the built-in optimizations and conventions provided by `next/navigation`.
  When valid: For extremely simple applications with minimal URL interaction.

## Risks

- Breaking changes in future versions of `react` or `next/navigation`.
  Mitigation: Regularly review release notes and update dependencies incrementally with thorough testing.
  Owner: Engineering Team
- Performance degradation due to excessive client-side rendering.
  Mitigation: Profile component rendering, optimize state updates, and strategically use server components where interactivity is not required.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure proper hydration of client components to avoid flickering or re-rendering issues.
- Consider using memoization techniques (e.g., `React.memo`, `useCallback`, `useMemo`) to optimize performance of client-side components.
- When interacting with `searchParams`, be mindful of potential client-side vs. server-side discrepancies and ensure consistent behavior.

## Continuation Context


Verify commands:
- Inspect the project's dependency manifest to identify declared `react` and `next/navigation` versions.
- Examine the project's lock file to confirm the exact resolved versions of `react` and `next/navigation`.
- Review client-side component files for the presence of `"use client"` directives and usage of `useState`, `useEffect`, and `searchParams.get()`.

Accept when:
- The project's dependency resolution for `react` and `next/navigation` is consistent and locked.
- Interactive UI components correctly manage state using `useState` and `useEffect`.
- URL query parameters are accurately read and reflected in client-side components via `next/navigation`.
- Client-side components are appropriately marked with `"use client"`.

## Enforcement

- Verified by: Code reviews, automated linting rules, and integration tests.
- Violation handling: Violations will be flagged during code review or by CI/CD pipelines, requiring remediation before merging.
- Exception process: Exceptions require explicit approval from a lead architect, documented with a clear rationale and potential alternative solutions.