# Adoption of clsx for Conditional Class Name Management: Prefer Clsx Utility Over Manual String

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- Dynamic styling is a common requirement in modern web applications.
- Managing conditional CSS classes can lead to verbose and error-prone code.
- The project utilizes component-based architecture where styling is often applied directly within components.

## Problem Statement

Manually concatenating CSS class strings based on conditional logic within UI components can lead to unreadable, difficult-to-maintain, and error-prone code, especially as component complexity grows.

## Decision

1. SHOULD: Prefer the `clsx` utility over manual string interpolation or template literals for conditional class name generation.

## Policy Block

- SHOULD Prefer the `clsx` utility over manual string interpolation or template literals for conditional class name generation.

In scope:
- Any UI component requiring conditional CSS class application.

Out of scope:
- Static CSS class definitions that do not require dynamic evaluation.

## Rationale

- The `clsx` utility provides a concise and readable API for combining class names conditionally.
- It reduces boilerplate code associated with dynamic styling.
- Its widespread use across existing UI components indicates established practice and familiarity within the codebase.

## Consequences

Positive:
- Improved readability and maintainability of component styling logic.
- Reduced errors in class name generation.
- Consistent approach to dynamic styling across the codebase.

Negative:
- Introduction of an additional third-party dependency.
- Potential for misuse if not applied consistently.

## Alternatives

- Manual string concatenation (rejected)
  Rejected because: Leads to verbose, error-prone, and less readable code.
  When valid: For very simple, non-conditional class assignments.
- Template literals for conditional class names (rejected)
  Rejected because: Can still become unwieldy with complex conditions compared to the `clsx` utility.
  When valid: For simple, single-condition class assignments.

## Risks

- Over-reliance on the `clsx` utility for all styling, potentially obscuring simpler CSS solutions.
  Mitigation: Encourage review of styling complexity to ensure the `clsx` utility is used appropriately.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Developers should consult existing component implementations for examples of `clsx` utility usage.

## Continuation Context


Verify commands:
- Inspect component files for instances of class name concatenation.
- Run the project's linting and testing suite to identify any styling inconsistencies.

Accept when:
- All new and modified UI components use the `clsx` utility for dynamic class name generation.
- Code reviews confirm consistent application of the `clsx` utility where appropriate.
- No new instances of manual conditional string concatenation for class names are introduced.

## Enforcement

- Verified by: Code reviews
- Verified by: Automated linting rules
- Violation handling: Code review comments
- Violation handling: Linting failures blocking merges
- Exception process: Documented exception approved by a lead engineer, with clear justification.