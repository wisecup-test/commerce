# Client-Side UI Interaction with React Hooks: Before Implementing Any Code That Uses

Status: proposed
Date: 2023-10-27
Deciders: Detection Pipeline (automated)

## Context

- The application utilizes `react` for building user interfaces.
- Interactive components require local state management and side effects.
- Client-side rendering is necessary for dynamic UI behavior and browser API access.
- The codebase frequently employs `useState` and `useEffect` for these purposes.

## Decision

1. MUST: Before implementing any code that uses a versioned library, the exact resolved version of that library MUST be identified from the project's lock file or resolution artifact.

## Policy Block

- MUST Before implementing any code that uses a versioned library, the exact resolved version of that library MUST be identified from the project's lock file or resolution artifact.

In scope:
- Interactive UI components
- Client-side rendered components

Out of scope:
- Server-side rendered components without interactivity
- Purely presentational components without state or side effects

## Rationale

- `useState` provides a standard and efficient mechanism for managing component-specific mutable state.
- `useEffect` centralizes side effect logic, improving readability and preventing common issues like race conditions or memory leaks.
- The `use client` directive explicitly marks components for client-side execution, ensuring access to browser APIs and interactive capabilities.
- This pattern aligns with the observed usage across multiple interactive components, indicating a consistent approach.

## Consequences

Positive:
- Consistent state management and side effect handling across interactive UI components.
- Improved component reusability and maintainability due to standardized hook usage.
- Clear demarcation of client-side rendered components, aiding in build optimization and understanding.

Negative:
- Potential for performance issues if `useEffect` dependencies are not managed carefully, leading to unnecessary re-renders.
- Increased bundle size for client-side components compared to purely server-rendered ones.

## Alternatives

- Class Components with `this.state` and lifecycle methods. (rejected)
  Rejected because: The codebase consistently uses functional components and hooks, indicating a preference for the modern React paradigm.
  When valid: For legacy components that are not being actively refactored.
- External state management libraries (e.g., Redux, Zustand). (rejected)
  Rejected because: While suitable for global state, the evidence shows `useState` and `useEffect` are preferred for local, component-specific interactions.
  When valid: For managing global application state or complex state interactions across many components.

## Risks

- Misuse of `useEffect` leading to infinite loops or inefficient re-renders.
  Mitigation: Thorough code reviews and adherence to `useEffect` dependency array best practices.
  Owner: engineering team
- Over-reliance on client-side rendering for non-interactive content, impacting initial page load performance.
  Mitigation: Strategic use of the `use client` directive only where interactivity or browser APIs are strictly required.
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
- Ensure proper cleanup functions are returned from `useEffect` to prevent memory leaks for subscriptions or event listeners.
- Consider using custom hooks to encapsulate complex stateful logic and side effects for reusability.

## Continuation Context


Verify commands:
- Inspect component files for the presence of `useState` and `useEffect` imports from the `react` library.
- Verify that interactive client-side components include the `use client` directive at the top of the file.
- Examine the project's dependency lock file to confirm the resolved version of the `react` library.

Accept when:
- `useState` and `useEffect` are consistently used for local state and side effects in interactive components.
- The `use client` directive is present in all components requiring client-side interactivity.
- The resolved `react` version is documented and consistent with project standards.

## Enforcement

- Verified by: Automated linting rules, code reviews, and static analysis tools.
- Violation handling: Code failing to adhere to these rules will be flagged during code review or by CI/CD pipelines, requiring remediation before merging.
- Exception process: Exceptions require explicit approval from a lead engineer, documented with a clear rationale and potential alternatives.