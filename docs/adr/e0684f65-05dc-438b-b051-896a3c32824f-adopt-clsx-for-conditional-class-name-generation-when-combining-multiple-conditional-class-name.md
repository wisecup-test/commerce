# Adopt `clsx` for Conditional Class Name Generation: When Combining Multiple Conditional Class Name

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- UI components often require dynamic class names based on component state or props.
- Concatenating class name strings conditionally can become verbose and error-prone.
- A consistent approach to managing conditional classes improves readability and maintainability.

## Problem Statement

The project needs a standardized and efficient method for constructing conditional CSS class strings within UI components to avoid manual string concatenation and improve code clarity.

## Decision

1. SHOULD: When combining multiple conditional class name expressions, `clsx` SHOULD be preferred over manual template literals or ternary operators for clarity.

## Policy Block

- SHOULD When combining multiple conditional class name expressions, `clsx` SHOULD be preferred over manual template literals or ternary operators for clarity.

In scope:
- UI components that require conditional styling based on dynamic conditions.

Out of scope:
- Static class name declarations or styling solutions that do not involve conditional class name logic.

## Rationale

- `clsx` provides a concise and readable API for conditionally combining class names, reducing boilerplate.
- Its adoption standardizes class name construction across the codebase, improving consistency.
- The utility handles various input types (strings, objects, arrays) gracefully, simplifying complex conditional logic.

## Consequences

Positive:
- Improved readability and maintainability of UI component styling logic.
- Reduced errors associated with manual conditional class string concatenation.
- Consistent approach to dynamic styling across the codebase.

Negative:
- Adds a minor dependency to the project.
- Developers new to `clsx` may require a brief learning period.

## Alternatives

- Manual string concatenation or template literals. (rejected)
  Rejected because: Leads to verbose, error-prone code, especially with complex conditional logic.
  When valid: For very simple, single-condition class names where `clsx` might be overkill.
- Another class name utility library (e.g., `classnames`). (rejected)
  Rejected because: `clsx` is already observed in the codebase, and introducing another similar utility would fragment the approach.
  When valid: If `clsx` were found to have significant limitations not present in alternatives.

## Risks

- Over-reliance on `clsx` for all styling, potentially obscuring complex styling logic.
  Mitigation: Encourage clear separation of concerns, using `clsx` only for conditional class name joining, not for complex style object manipulation.
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
- Ensure `clsx` is imported consistently across files.
- Consider creating a linting rule to enforce `clsx` usage for conditional class names.

## Continuation Context


Verify commands:
- Inspect relevant UI component files for imports of class name utility functions.
- Review pull requests for new or modified UI components to ensure `clsx` is used for conditional class names.
- Run the project's test suite to ensure no regressions in styling due to class name changes.

Accept when:
- All UI components requiring conditional class names use the adopted utility.
- No manual string concatenations for conditional class names are found in new code.
- The project's build and test processes complete successfully.

## Enforcement

- Verified by: Automated CI checks for dependency usage and code reviews.
- Violation handling: Code reviews will flag non-compliant code; CI may fail builds if linting rules are established.
- Exception process: Exceptions require explicit approval from a lead engineer, documented with a clear rationale.