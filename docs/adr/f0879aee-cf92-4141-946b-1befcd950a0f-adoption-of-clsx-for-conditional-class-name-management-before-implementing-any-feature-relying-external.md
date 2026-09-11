# Adoption of clsx for Conditional Class Name Management: Before Implementing Any Feature Relying External

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The project requires dynamic styling of UI components based on various conditions and props.
- There is a need for a clean and readable way to concatenate multiple class names, especially when some are conditional.
- Manual string concatenation or complex template literals for class names can become cumbersome and error-prone.

## Decision

1. MUST: Before implementing any feature relying on an external dependency, developers MUST verify the exact resolved version of that dependency using the project's lock file.

## Policy Block

- MUST Before implementing any feature relying on an external dependency, developers MUST verify the exact resolved version of that dependency using the project's lock file.

In scope:
- UI components requiring conditional styling
- Modules concatenating multiple class names

Out of scope:
- Simple static class name assignments
- Styling managed purely by CSS modules or styled-components without conditional logic

## Rationale

- The `clsx` utility provides a concise and highly readable API for combining class names, significantly improving the clarity of styling logic.
- Its adoption simplifies conditional logic for styling, reducing the likelihood of errors compared to manual string manipulation.
- The pattern is observed in existing UI components, indicating an established and effective usage within the codebase.

## Consequences

Positive:
- Improved readability and maintainability of component styling logic.
- Reduced boilerplate and complexity for managing conditional class names.
- Consistent approach to class name management across the codebase.

Negative:
- Introduces a minor additional dependency to the project.
- Requires developers to be familiar with the `clsx` API.

## Alternatives

- Manual string concatenation or template literals for class names. (rejected)
  Rejected because: This approach leads to less readable and more error-prone code for complex conditional styling, especially with multiple conditions.
  When valid: For very simple, static class name assignments without any conditional logic.
- Using a different utility library for class name concatenation. (rejected)
  Rejected because: `clsx` is already adopted and widely used within the project; introducing another similar utility would add unnecessary complexity and inconsistency.
  When valid: If `clsx` is found to not meet a specific, advanced requirement that another utility fulfills better.

## Risks

- Potential for minor dependency bloat due to the additional utility library.
  Mitigation: Regularly review project dependencies and their impact on bundle size and performance.
  Owner: Engineering team
- Developers unfamiliar with `clsx` may initially struggle with its usage.
  Mitigation: Provide clear documentation and code examples for `clsx` usage within the project guidelines.
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
- `clsx` can accept multiple arguments, including objects where keys are class names and values are booleans, providing flexible conditional logic.
- Developers should prioritize using `clsx` over manual string concatenation for any conditional class logic to maintain consistency and readability.

## Continuation Context


Verify commands:
- Inspect the project's dependency manifest for class name utility libraries.
- Examine UI component files for conditional class name assignments.
- Run the project's test suite to ensure styling behaves as expected.

Accept when:
- The dependency manifest includes a class name utility library.
- UI components consistently use the adopted utility for conditional class names.
- All tests related to component styling pass without errors.

## Enforcement

- Verified by: Automated linting rules
- Verified by: Code reviews
- Violation handling: Violations are flagged during code review and require correction.
- Violation handling: Linting errors prevent code from being committed or built.
- Exception process: Requires explicit approval from the architectural review board with documented justification for deviation.