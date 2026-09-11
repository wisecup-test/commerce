# Adoption of React Client Components for Interactive UI: Any Module That Contains React Client

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The project utilizes a React Server Components architecture.
- Certain UI components require client-side interactivity, state management, or browser-specific APIs.
- A hybrid rendering approach is desired to balance performance and interactivity.
- The "use client" directive is consistently applied to components needing client-side features.

## Decision

1. MUST: Any module that contains React Client Components MUST use the "use client" directive at its top.

## Policy Block

- MUST Any module that contains React Client Components MUST use the "use client" directive at its top.

In scope:
- Any React component requiring client-side interactivity or state.
- Any React component utilizing browser-specific APIs or event handlers.

Out of scope:
- Purely static React components.
- React components that can be fully rendered on the server without client-side hydration.

## Rationale

- The "use client" directive explicitly marks components for client-side rendering, enabling interactivity.
- This approach allows for a hybrid rendering model, optimizing initial page load with Server Components while providing rich user experiences with Client Components.
- Separating client and server concerns improves maintainability and performance characteristics.

## Consequences

Positive:
- Enables rich, interactive user interfaces.
- Optimizes initial page load by offloading static rendering to the server.
- Clear separation of client and server concerns in the codebase.

Negative:
- Increased bundle size for client components compared to pure server components.
- Potential for hydration mismatches if server and client render different content.
- Requires careful consideration of component boundaries and data flow between server and client.

## Alternatives

- Render all components on the server. (rejected)
  Rejected because: Would prevent any client-side interactivity, state management, or use of browser APIs.
  When valid: For purely static websites with no dynamic user interaction.
- Render all components on the client (traditional SPA). (rejected)
  Rejected because: Would lose the performance benefits of server-side rendering and React Server Components.
  When valid: For applications where initial load performance is less critical than full client-side control and complex state management.

## Risks

- Overuse of "use client" leading to larger client bundles and reduced performance benefits.
  Mitigation: Regular code reviews and performance monitoring to identify and refactor components that could be Server Components.
  Owner: Engineering Team.
- Hydration errors due to mismatches between server-rendered and client-rendered content.
  Mitigation: Thorough testing, especially end-to-end tests, and careful development practices when mixing server and client components.
  Owner: Engineering Team.

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure that any data passed from a Server Component to a Client Component is serializable.
- Consider the performance implications of moving components from server to client and vice-versa.

## Continuation Context


Verify commands:
- Inspect component files for the "use client" directive at the top of modules requiring interactivity.
- Run the project's build process and observe the generated client-side bundles for components marked "use client".
- Execute the project's UI tests to ensure client-side interactivity functions as expected.

Accept when:
- The "use client" directive is correctly placed in interactive components.
- Client-side functionality (e.g., state updates, event handling) works without errors.
- The application maintains a good balance of server-side rendering for static content and client-side rendering for dynamic interactions.

## Enforcement

- Verified by: Code reviews, automated linting rules, and end-to-end tests.
- Violation handling: Code review comments, automated build failures for linting violations.
- Exception process: Documented architectural review and approval process for deviations.