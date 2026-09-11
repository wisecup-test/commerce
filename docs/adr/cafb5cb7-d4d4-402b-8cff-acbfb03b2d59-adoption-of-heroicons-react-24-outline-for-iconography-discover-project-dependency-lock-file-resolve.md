# Adoption of @heroicons/react/24/outline for Iconography: Discover Project Dependency Lock File Resolve

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- UI components require a consistent set of vector icons to enhance user experience and provide visual cues.
- The project utilizes React for building its user interface.
- A standardized approach to iconography reduces design inconsistencies and improves developer workflow.
- Other core libraries like clsx, components/cart/actions, lib/shopify/types, and next/navigation are also used in these UI components.

## Problem Statement

Ensuring a consistent and efficient method for integrating scalable vector icons into React components across the application.

## Decision

1. MUST: Discover the project's dependency lock file and resolve the exact locked version of @heroicons/react/24/outline before implementation.

## Policy Block

- MUST Discover the project's dependency lock file and resolve the exact locked version of @heroicons/react/24/outline before implementation.

In scope:
- React components requiring vector icons.
- UI components within the components/cart directory.

Out of scope:
- Components not requiring vector icons.
- Non-React parts of the application.

## Rationale

- @heroicons/react/24/outline provides a high-quality, consistent set of SVG icons optimized for React.
- Direct component usage simplifies integration and styling within the React ecosystem.
- Standardizing on one icon library reduces bundle size and avoids visual inconsistencies.

## Consequences

Positive:
- Consistent visual design across the application.
- Improved developer efficiency due to a single, well-documented icon source.
- Reduced maintenance overhead for iconography.

Negative:
- Dependency on a third-party library for iconography.
- Potential for increased bundle size if not tree-shaken effectively.

## Alternatives

- Custom SVG icons. (rejected)
  Rejected because: Requires manual creation and optimization of SVGs, leading to higher development cost and potential inconsistency.
  When valid: For highly custom or branded icons not available in standard libraries.
- Font icon libraries (e.g., Font Awesome). (rejected)
  Rejected because: Font icons can have rendering issues, accessibility concerns, and are less flexible for styling compared to SVG components.
  When valid: For legacy projects or specific performance needs where SVG overhead is a concern.

## Risks

- Library deprecation or lack of maintenance.
  Mitigation: Regularly review library health and community support; plan for migration if necessary.
  Owner: Engineering team.
- Performance impact due to large icon set.
  Mitigation: Utilize tree-shaking and only import necessary icons; lazy load icon components if performance becomes an issue.
  Owner: Engineering team.

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure proper accessibility attributes (e.g., aria-label) are applied to icon components where appropriate.
- Consider creating a wrapper component for common icon usage patterns to abstract away direct library imports.

## Continuation Context


Verify commands:
- Inspect the project's dependency manifest for the presence of the icon library.
- Search the codebase for imports of the icon library within React components.
- Review UI components to confirm icons are rendered using the specified library's components.

Accept when:
- The icon library is listed as a dependency in the project's manifest.
- Icon components from the library are found in relevant UI files.
- Rendered UI displays consistent iconography.

## Enforcement

- Verified by: Code reviews, automated linting rules.
- Violation handling: Code changes will be rejected until compliance is met.
- Exception process: Requires explicit approval from a lead architect, documented with a clear rationale for deviation.