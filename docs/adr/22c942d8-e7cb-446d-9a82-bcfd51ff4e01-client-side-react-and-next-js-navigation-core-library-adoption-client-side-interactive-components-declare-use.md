# Client-Side React and Next.js Navigation Core Library Adoption: Client Side Interactive Components Declare Use

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The project requires interactive user interfaces that respond to user input and dynamic data.
- React is adopted as the primary library for building UI components.
- Next.js is used as the framework, necessitating interaction with its routing and rendering mechanisms.
- Specific type definitions are provided by `lib/shopify/types` to ensure data consistency and developer experience.

## Decision

1. MUST: Client-side interactive components MUST declare "use client" at the top of the file.

## Policy Block

- MUST Client-side interactive components MUST declare "use client" at the top of the file.

In scope:
- Client-side React components
- Next.js application pages requiring client-side interactivity
- Modules importing `react` or `next/navigation`

Out of scope:
- Server-side React components
- API routes
- Utility functions without UI interaction

## Rationale

- Explicitly marking components with "use client" enables rich interactive user experiences by allowing browser-specific APIs and hooks.
- Utilizing `useMemo` and efficient data access patterns optimizes client-side performance and reduces unnecessary re-renders.
- Adopting `next/navigation`'s `searchParams.get()` provides a consistent and idiomatic way to interact with Next.js routing features for client-side parameter access.
- The use of `lib/shopify/types` ensures type safety and consistency across data structures related to the application's domain.

## Consequences

Positive:
- Improved user experience through highly interactive and responsive UI components.
- Enhanced performance for interactive elements due to memoization and optimized data handling.
- Consistent development patterns for client-side logic and navigation within the Next.js framework.
- Clear separation of client and server concerns, aiding in maintainability and scalability.

Negative:
- Increased client-side JavaScript bundle size for components marked with "use client".
- Potential for hydration mismatches if server-rendered HTML does not perfectly align with client-side component structure.
- Reliance on specific library versions, requiring careful management during upgrades to avoid breaking changes.

## Alternatives

- Exclusive use of Server Components (rejected)
  Rejected because: Limits the ability to create highly interactive and dynamic user interfaces that require browser-specific APIs and client-side state management.
  When valid: For purely static or server-rendered content without client-side interactivity.
- Custom client-side routing and state management solutions (rejected)
  Rejected because: Introduces unnecessary complexity and re-implements functionality already provided and optimized by Next.js and React's built-in features.
  When valid: In projects not using Next.js or React, or with highly specialized, non-standard routing requirements.

## Risks

- Performance degradation due to excessive or inefficient client-side rendering and hydration.
  Mitigation: Strict adherence to performance optimization techniques, including judicious use of `useMemo` and minimizing client-side component scope. Regular performance profiling.
  Owner: Engineering team
- Breaking changes or unexpected behavior during upgrades of `react` or `next/navigation`.
  Mitigation: Thorough testing of client-side components after library upgrades. Utilizing lock files to pin exact dependency versions and reviewing changelogs.
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
- Ensure proper hydration by carefully matching the server-rendered HTML structure with the client-side component's expected output to prevent re-rendering issues.
- Minimize the scope of components marked with "use client" to only those that strictly require client-side features, thereby reducing the overall client-side bundle size.

## Continuation Context


Verify commands:
- Discover the project's build command and execute it.
- Discover the project's test command and execute it.
- Discover the project's lint command and execute it.

Accept when:
- The build process completes without errors.
- All automated tests pass successfully.
- Linting reports no critical issues or warnings related to client-side component usage.

## Enforcement

- Verified by: Automated CI checks for build success, test pass rates, and linting compliance.
- Verified by: Code reviews ensuring adherence to "use client" placement and `useMemo` usage.
- Verified by: Runtime monitoring for performance regressions in client-side components.
- Violation handling: Code rejected in pull request if it violates established rules.
- Violation handling: Automated build failure in CI/CD pipeline.
- Violation handling: Performance alerts triggered by runtime monitoring.
- Exception process: Exceptions to these rules require documented approval from the Architectural Review Board, detailing the specific justification and mitigation strategies.