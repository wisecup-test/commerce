# URL Search Parameter Management with Next.js `searchParams`: When Modifying Url Search Parameters Components

Status: proposed
Date: 2023-10-27
Deciders: Detection Pipeline (automated)

## Context

- Applications require state to be reflected in the URL for shareability and navigation.
- Next.js provides specific APIs for interacting with URL search parameters.
- Components need a consistent way to read and modify query parameters.
- Server-side components and client-side components both interact with URL state.

## Problem Statement

How to consistently and effectively manage URL search parameters across various components to ensure state persistence, shareability, and proper integration with Next.js routing.

## Decision

1. MUST: When modifying URL search parameters, components MUST construct new `URLSearchParams` instances to ensure immutability and proper state updates.

## Policy Block

- MUST When modifying URL search parameters, components MUST construct new `URLSearchParams` instances to ensure immutability and proper state updates.

In scope:
- Client-side components requiring URL state management
- Server-side components accessing incoming URL query parameters

Out of scope:
- Internal application state not reflected in the URL
- Routing logic not involving query parameters

## Rationale

- Direct use of `searchParams` and `URLSearchParams` aligns with Next.js best practices for URL state management.
- Ensures consistent behavior and predictable state updates across the application.
- Facilitates server-side rendering and client-side hydration of URL-driven state.
- Leverages built-in framework capabilities for robustness.

## Consequences

Positive:
- Improved URL shareability.
- Better browser history integration.
- Consistent state management.
- Easier debugging of URL-driven state.

Negative:
- Potential for boilerplate code when constructing new `URLSearchParams` objects.
- Developers must be aware of client vs. server context for `searchParams` access.

## Alternatives

- Manual parsing of `window.location.search` string. (rejected)
  Rejected because: Prone to errors, lacks framework integration, and requires manual serialization/deserialization.
  When valid: For very simple, static pages without a framework.
- Using a third-party URL state management library. (rejected)
  Rejected because: Introduces additional dependency overhead and complexity when Next.js provides adequate built-in solutions.
  When valid: For highly complex URL state requirements not met by native APIs.

## Risks

- Inconsistent parameter naming leading to confusion.
  Mitigation: Establish and document a naming convention for common query parameters.
  Owner: Engineering Team.
- Accidental direct mutation of `searchParams` object (if not using `URLSearchParams` correctly).
  Mitigation: Code reviews and linting rules to enforce immutability when updating URL state.
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
- Remember that `searchParams` from `next/navigation` is read-only and requires creating a new `URLSearchParams` instance for modifications.
- Consider creating utility functions to abstract common URL parameter manipulation patterns.

## Continuation Context


Verify commands:
- Inspect relevant component files for usage of URL search parameter APIs.
- Run the application's test suite to ensure URL state transitions are correct.
- Manually test application features that rely on URL query parameters.

Accept when:
- All components correctly read URL search parameters.
- URL modifications result in expected application state changes and navigation.
- No direct mutations of the `searchParams` object are observed without proper `URLSearchParams` construction.

## Enforcement

- Verified by: Code reviews, automated linting rules, integration tests.
- Violation handling: Code will be rejected during review; automated checks will fail CI builds.
- Exception process: Exceptions require explicit approval from a lead engineer, documented with a clear rationale.