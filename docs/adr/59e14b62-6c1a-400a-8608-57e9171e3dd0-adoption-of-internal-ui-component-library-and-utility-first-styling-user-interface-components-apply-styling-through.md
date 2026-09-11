# Adoption of Internal UI Component Library and Utility-First Styling: User Interface Components Apply Styling Through

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The project requires a consistent and reusable set of UI elements across different pages.
- Efficient styling and responsive design are critical for a good user experience.
- Development speed benefits from pre-built and standardized UI components.
- The codebase exhibits repeated usage of specific component paths and styling patterns.

## Problem Statement

The project needs a standardized approach for building user interfaces that ensures consistency, reusability, and maintainability across various application pages, while also supporting efficient and responsive styling.

## Decision

1. MUST: User interface components MUST apply styling through utility classes, adhering to the established utility-first CSS conventions.

## Policy Block

- MUST User interface components MUST apply styling through utility classes, adhering to the established utility-first CSS conventions.

In scope:
- User interface components
- Page-level layouts
- Styling definitions

Out of scope:
- Backend services
- Data fetching logic
- Non-visual utility functions

## Rationale

- The observed pattern of using shared components and utility classes indicates a deliberate choice for UI consistency and development efficiency.
- Centralizing UI components promotes reusability and reduces duplication across the codebase.
- Utility-first styling enables rapid UI development and consistent application of design tokens.
- Standardizing UI development practices improves maintainability and onboarding for new developers.

## Consequences

Positive:
- Increased UI consistency and brand adherence across the application.
- Faster development cycles for new UI features due to component reusability.
- Improved maintainability of the user interface codebase.
- Enhanced collaboration among frontend developers through shared patterns.

Negative:
- Potential for a steeper learning curve for developers unfamiliar with the specific component library or utility-first CSS approach.
- Over-reliance on utility classes might lead to verbose JSX if not managed properly.
- Maintenance overhead for the internal UI component library itself.

## Alternatives

- Use a third-party UI component library (e.g., Material UI, Ant Design). (rejected)
  Rejected because: The project has already invested in and established an internal component library, indicating a preference for custom control and branding.
  When valid: For projects starting from scratch without specific branding requirements.
- Implement component-specific CSS modules or CSS-in-JS. (rejected)
  Rejected because: The observed pattern strongly favors utility-first classes for styling, suggesting a preference for atomic CSS principles and direct application in JSX.
  When valid: For highly complex, isolated components requiring deep encapsulation or dynamic styling not easily achieved with utility classes.

## Risks

- Stagnation or lack of maintenance for the internal UI component library.
  Mitigation: Assign dedicated ownership and allocate regular time for component library development and updates.
  Owner: Engineering Team
- Inconsistent application of utility classes leading to visual discrepancies.
  Mitigation: Implement linting rules and code reviews to enforce consistent utility class usage.
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
- Developers should consult the internal documentation for available UI components and their usage guidelines.
- New utility classes should be added to the project's configuration only after review and approval by the UI lead.

## Continuation Context


Verify commands:
- Discover and execute the project's UI component storybook or documentation generation script.
- Run the project's visual regression tests.
- Inspect the project's build output for unused CSS classes.

Accept when:
- All UI components render correctly according to design specifications.
- Visual regression tests pass without unexpected changes.
- No critical CSS issues or unused styles are reported by build tools.

## Enforcement

- Verified by: Code reviews, automated linting, and visual regression testing in CI/CD pipelines.
- Violation handling: Violations will result in code review comments requiring remediation or CI/CD pipeline failures.
- Exception process: Exceptions require explicit approval from the UI lead or architectural review board, with documented justification.