# Client Component Directive ("use client") in React: Ensure That Components Marked Use Client

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The project utilizes React for UI development.
- The architecture incorporates a rendering model that distinguishes between server and client components.
- Certain UI components require client-side interactivity and state management.
- Access to browser-specific APIs or hooks like useState and useEffect necessitates client-side execution.

## Problem Statement

In a React Server Components architecture, a mechanism is needed to explicitly designate components that must execute on the client-side to enable interactivity, state management, and access to browser-specific APIs, while maintaining the benefits of server-side rendering for other components.

## Decision

1. MUST: Ensure that components marked with "use client" are only imported by other client components or passed as props from server components.

## Policy Block

- MUST Ensure that components marked with "use client" are only imported by other client components or passed as props from server components.

In scope:
- React components requiring client-side interactivity
- Components using useState or useEffect
- Components accessing browser-specific APIs like searchParams

Out of scope:
- Purely presentational components without client-side interactivity
- Components that only render static content
- Server components that do not directly consume client-side hooks or APIs

## Rationale

- The "use client" directive explicitly signals to the rendering framework that a component and its children should be rendered on the client, enabling interactive features.
- This approach allows for a hybrid rendering model, optimizing initial page load and performance by rendering static parts on the server while providing rich interactivity where needed.
- Separating client and server concerns improves maintainability and reduces the bundle size sent to the client.

## Consequences

Positive:
- Improved user experience through interactive UI elements
- Optimized initial page load performance
- Clear separation of client and server concerns

Negative:
- Increased client-side bundle size for components marked "use client"
- Potential for hydration errors if server and client trees mismatch
- Requires careful consideration of component boundaries

## Alternatives

- Render all components on the server (rejected)
  Rejected because: Does not allow for client-side interactivity, state management, or access to browser APIs.
  When valid: For purely static websites or applications with no user interaction.
- Render all components on the client (traditional SPA) (rejected)
  Rejected because: Sacrifices initial page load performance and SEO benefits of server-side rendering.
  When valid: For highly interactive applications where initial load time is less critical than client-side rendering flexibility.

## Risks

- Overuse of "use client" leading to larger client bundles and reduced performance benefits.
  Mitigation: Regularly audit client components and refactor to push as much as possible to server components.
  Owner: Engineering Team
- Hydration mismatches between server-rendered HTML and client-side React.
  Mitigation: Thorough testing and careful management of client-side state initialization.
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
- Place the "use client" directive at the very top of the file, before any imports.
- Consider passing interactive elements as children to server components to minimize the client bundle.

## Continuation Context


Verify commands:
- Discover the project's build command and run it to ensure no client/server component boundary errors are reported.
- Discover the project's test command and execute it to confirm client component functionality.
- Discover the project's linter command and run it to check for proper directive placement.

Accept when:
- The build process completes without errors related to client/server component boundaries.
- All client-side interactive features function as expected in the browser.
- Linter checks pass, ensuring correct "use client" directive usage.

## Enforcement

- Verified by: CI/CD pipelines, code reviews, automated linting.
- Violation handling: Build failures, code review comments, automated linter warnings.
- Exception process: Documented architectural review and approval process for deviations.