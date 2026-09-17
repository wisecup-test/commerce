# Adoption of lib/shopify and lib/shopify/types Modules: Exact Resolved Version Any Versioned Dependency

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The project utilizes a set of core libraries for various functionalities.
- Specific modules (`lib/shopify`, `lib/shopify/types`) are observed for handling Shopify-related data and logic.
- UI components require structured data types and consistent interaction patterns with external services.
- Client-side rendering and navigation are integral to the application's user experience.

## Problem Statement

Ensuring consistent and type-safe interaction with Shopify data and services across various UI components, while maintaining a clear separation of concerns and leveraging established core libraries.

## Decision

1. MUST: The exact resolved version of any versioned dependency MUST be verified by inspecting the project's lock file before implementation.

## Policy Block

- MUST The exact resolved version of any versioned dependency MUST be verified by inspecting the project's lock file before implementation.

In scope:
- All UI components interacting with Shopify data.
- All modules responsible for fetching or manipulating Shopify-related information.

Out of scope:
- General utility modules not directly involved with Shopify.
- UI components that do not display or interact with Shopify data.

## Rationale

- Centralizes Shopify-related data types and logic, improving maintainability and consistency.
- Provides a clear abstraction layer over the Shopify API, reducing direct dependencies on external SDKs in UI components.
- Enforces type safety for Shopify data, reducing runtime errors and improving developer experience.
- Facilitates easier updates or changes to the Shopify integration by localizing modifications.

## Consequences

Positive:
- Improved code readability and maintainability for Shopify integrations.
- Reduced boilerplate code in UI components for data handling.
- Enhanced type safety and fewer runtime errors related to Shopify data.
- Easier onboarding for new developers working on Shopify features.

Negative:
- Increased initial overhead for defining types and wrappers in `lib/shopify/types` and `lib/shopify`.
- Potential for tight coupling between UI components and the `lib/shopify` modules if not carefully managed.

## Alternatives

- Directly use Shopify's official SDKs or GraphQL API clients in UI components. (rejected)
  Rejected because: Leads to scattered Shopify logic, reduced type safety without explicit wrappers, and increased coupling of UI to external APIs.
  When valid: For very small projects with minimal Shopify interaction where the overhead of an abstraction layer is not justified.
- Define Shopify data types and interaction logic ad-hoc within each component. (rejected)
  Rejected because: Results in code duplication, inconsistency, and makes refactoring or updates extremely difficult.
  When valid: Never for a project of this observed complexity.

## Risks

- `lib/shopify` and `lib/shopify/types` modules become overly complex or monolithic.
  Mitigation: Regularly refactor and organize the modules into smaller, focused sub-modules or services.
  Owner: engineering team
- Outdated or incorrect type definitions in `lib/shopify/types` lead to inconsistencies with the actual Shopify API.
  Mitigation: Implement automated schema generation or validation against the Shopify API, and ensure regular updates.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- New Shopify-related features should start by defining necessary types in `lib/shopify/types` before implementing logic.
- Contributions to `lib/shopify` should prioritize reusability and clear API contracts for consuming components.

## Continuation Context


Verify commands:
- Inspect the project's dependency manifest to identify declared library ranges.
- Examine the project's lock file to determine the exact resolved versions of core libraries.
- Run the project's test suite to ensure existing integrations with `lib/shopify` and `lib/shopify/types` function correctly.

Accept when:
- The project's build process completes without errors related to Shopify integration.
- All unit and integration tests for components using `lib/shopify` and `lib/shopify/types` pass.
- Code reviews confirm adherence to the prescribed usage patterns for these modules.

## Enforcement

- Verified by: CI/CD pipelines
- Verified by: code reviews
- Violation handling: Automated build failures
- Violation handling: mandatory code review comments requiring remediation
- Exception process: Requires explicit approval from a lead architect or designated module owner, documented with a clear rationale.