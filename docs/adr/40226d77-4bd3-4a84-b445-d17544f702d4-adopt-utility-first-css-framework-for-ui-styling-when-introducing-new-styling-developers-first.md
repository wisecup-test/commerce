# Adopt Utility-First CSS Framework for UI Styling: When Introducing New Styling Developers First

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- UI components require consistent and efficient styling.
- Rapid development of new UI features necessitates a streamlined styling approach.
- The codebase exhibits direct application of styling via `className` attributes.
- Support for different themes (e.g., dark mode) is present.

## Problem Statement

Ensuring consistent, maintainable, and scalable styling across diverse UI components while facilitating rapid development and theme support.

## Decision

1. SHOULD: When introducing new styling, developers SHOULD first attempt to compose it from existing utility classes.

## Policy Block

- SHOULD When introducing new styling, developers SHOULD first attempt to compose it from existing utility classes.

In scope:
- All new and existing UI components that require visual styling.
- Components defining layout, typography, color, spacing, and responsive behaviors.

Out of scope:
- Styling concerns that cannot be adequately addressed by utility classes.
- Global base styles or resets that are typically handled at a higher level.

## Rationale

- The observed pattern of using `className` with numerous utility classes across multiple UI components indicates a deliberate adoption of a utility-first CSS framework.
- This approach promotes consistency by reusing predefined styles and accelerates development by reducing the need for custom CSS.
- Direct application of classes in markup simplifies component-level styling and reduces context switching.

## Consequences

Positive:
- Increased consistency in UI appearance due to reusable utility classes.
- Faster UI development cycles.
- Reduced CSS bundle size by avoiding custom, often redundant, styles.
- Simplified maintenance of component-specific styles.

Negative:
- HTML markup can become verbose with many `className` attributes.
- Steep learning curve for developers unfamiliar with utility-first CSS.
- Potential for 'class bloat' if not managed properly.

## Alternatives

- CSS Modules (rejected)
  Rejected because: The current codebase evidence strongly indicates a utility-first approach, not scoped CSS.
  When valid: For projects prioritizing strict component encapsulation and avoiding global style leakage.
- CSS-in-JS (e.g., Styled Components) (rejected)
  Rejected because: No evidence of JavaScript-based styling solutions in the observed pattern.
  When valid: For projects requiring dynamic, component-level styling with JavaScript logic.

## Risks

- Over-reliance on custom utility classes leading to inconsistency.
  Mitigation: Establish clear guidelines for extending the utility-first framework and review new class additions.
  Owner: Engineering Team.
- Difficulty in refactoring or changing global design tokens.
  Mitigation: Ensure design tokens are centrally defined and mapped to utility classes, allowing for single-point updates.
  Owner: Engineering Team.

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Developers should consult the framework's documentation for available utility classes and best practices for composition.
- Consider using tooling to sort and optimize `className` strings for readability and consistency.

## Continuation Context


Verify commands:
- Inspect UI component files for `className` attributes containing multiple utility classes.
- Run the project's build process and observe the generated CSS output for utility class definitions.
- Execute the project's UI tests to ensure visual consistency.

Accept when:
- New UI components are styled exclusively using utility classes.
- Existing UI components adhere to the utility-first styling pattern.
- The project's styling output is consistent with a utility-first framework.

## Enforcement

- Verified by: Code reviews.
- Verified by: Automated linting rules (if configured).
- Verified by: Visual regression testing.
- Violation handling: Code review comments requiring adherence to the utility-first styling pattern.
- Violation handling: Automated build failures for linting violations.
- Exception process: Exceptions require explicit approval from a designated architectural review board or lead frontend developer, documented with a clear rationale.