---
version: 2
name: b0e08545-2a5c-4a54-96ae-754c35cd6e3d-adoption-of-clsx-for-conditional-class-name-composition-components-not-implement-manual-string-concatenation
description: "Adoption of clsx for Conditional Class Name Composition: Components Not Implement Manual String Concatenation"
metadata:
  actual:
    adr_id: b0e08545-2a5c-4a54-96ae-754c35cd6e3d
    status: accepted
    candidate_group_id: e269d76a-28d5-498f-8cd2-9136c635a668
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of clsx for Conditional Class Name Composition: Components Not Implement Manual String Concatenation

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/**`

## Context

- Presentation components frequently require conditional styling based on component props, runtime parameters, and interaction state.
- Manual string interpolation and ternary expressions in class attributes introduce formatting defects, redundant whitespace, and readability degradation.
- Multiple user interface modules across the codebase exhibit a shared dependency on the clsx utility for assembling dynamic class strings.

## Problem Statement

Managing dynamic CSS class names through standard string concatenation or nested ternary expressions produces error-prone markup, irregular whitespace, and fragmented styling logic across user interface components. A uniform, lightweight utility is required to standardize conditional class resolution across all component boundaries without adding runtime overhead.

## Decision

1. MUST_NOT: Components MUST NOT implement manual string concatenation, array joins, or nested template literal ternaries for conditional class name evaluation.

## Policy Block

- MUST_NOT Components MUST NOT implement manual string concatenation, array joins, or nested template literal ternaries for conditional class name evaluation.

In scope:
- User interface components and presentation modules requiring conditional or dynamic class names.

Out of scope:
- Static style declarations that do not require runtime conditional toggling.
- Non-presentation utility modules that do not produce class name strings.

## Rationale

- Static analysis demonstrates widespread adoption of clsx across sixteen independent component files, proving an established structural pattern.
- The utility offers deterministic handling of truthy and falsy class inputs with minimal bundle weight compared to heavier class manipulation abstractions.
- Standardizing on a single utility eliminates idiosyncratic string manipulation patterns and reduces rendering defects in component markup.

## Consequences

Positive:
- Standardizes syntax for conditional styling across all presentational component boundaries.
- Eliminates formatting anomalies such as trailing spaces, undefined values, and extra delimiters in class attributes.
- Improves readability and maintainability of template markup across client and server rendering contexts.

Negative:
- Introduces an external library dependency across the component hierarchy.
- Requires team discipline and automated linting to prevent mixing utility usage with raw string concatenation.

## Alternatives

- Native template literal interpolation with ternary operators (rejected)
  Rejected because: Produces error-prone markup with nested conditions and frequently emits null or undefined values into class attributes
  When valid: Valid only in isolated environments where external dependencies are strictly prohibited
- In-house custom class joining helper function (rejected)
  Rejected because: Incurs ongoing maintenance overhead and re-implements edge-case handling for boolean and object arguments without performance benefits
  When valid: Valid in zero-dependency runtime environments where third-party packages cannot be resolved

## Risks

- Developers might bypass the utility and revert to ad-hoc string concatenation in newly created components.
  Mitigation: Automate verification using repository static analysis and enforce import checks during code review.
  Owner: Frontend Engineering Team
- Inadvertent inclusion of multiple conflicting class manipulation libraries in the project manifest.
  Mitigation: Enforce dependency boundary validation through automated repository manifest checks.
  Owner: Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Import the utility function consistently across component modules to handle dynamic class evaluation, avoiding ad-hoc helper implementations.
- Pass class names as distinct arguments or object entries where keys represent CSS class names and values represent boolean conditions.

## Continuation Context


Verify commands:
- sh -c 'echo "Discover and execute repository linting script to validate component class composition"'
- sh -c 'echo "Discover and execute repository build verification script to ensure dependency lock compliance"'

Accept when:
- The project static analysis and linting suite completes with zero violations across all UI components.
- All dynamic class name evaluations in component markup use clsx rather than manual string concatenation.
- The repository dependency resolution artifact confirms the locked utility version is resolved and active.

## Enforcement

- Verified by: Automated static analysis and lint checks executed during continuous integration.
- Verified by: Peer code review for all pull requests modifying UI components.
- Violation handling: Continuous integration pipeline failure when raw string concatenation or disallowed class joining patterns are detected.
- Violation handling: Mandatory refactoring of non-compliant class concatenation prior to merge approval.
- Exception process: Exceptions require formal architecture review and explicit documentation outlining the technical constraints that prevent utility adoption.