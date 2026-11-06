---
version: 2
name: 258a0fae-76d0-4409-8620-c727d8752f9e-adoption-of-price-component-module-for-standardized-monetary-presentation-consumer-components-pass-custom-visual-container
description: "Adoption of Price Component Module for Standardized Monetary Presentation: Consumer Components Pass Custom Visual Container"
metadata:
  actual:
    adr_id: 258a0fae-76d0-4409-8620-c727d8752f9e
    status: accepted
    candidate_group_id: da4c2516-68a9-4a63-8867-6fbbd796bc72
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - components/product/**
---

# Adoption of Price Component Module for Standardized Monetary Presentation: Consumer Components Pass Custom Visual Container

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/cart/**`
- `components/product/**`

## Context

- Multiple customer-facing surfaces require the display of monetary values across catalog detail views and cart inspection interfaces.
- Dispersed formatting logic across individual presentation modules introduces the risk of divergent currency representations, inconsistent decimal precision, and mismatched localized symbols.
- Static analysis identifies a shared dependency on a centralized internal Price component module across disparate presentation components.
- Consolidating monetary display rules into a reusable component module ensures uniform visual presentation and simplifies changes to internationalization policies.

## Problem Statement

Rendering currency values across disparate shopping surfaces without a centralized presentation module leads to fragmented formatting logic, inconsistent symbol placement, and maintenance overhead when internationalization or display requirements change.

## Decision

1. MAY: Consumer components MAY pass custom visual container styling properties to the shared Price component when surface-specific typography or spacing adjustments are required.

## Policy Block

- MAY Consumer components MAY pass custom visual container styling properties to the shared Price component when surface-specific typography or spacing adjustments are required.

In scope:
- User interface presentation components displaying product pricing, line item costs, or order subtotals
- Interactive shopping surfaces including product details and shopping cart interfaces

Out of scope:
- Backend data fetching, calculation, and checkout API integration routines that process raw numeric monetary values
- Non-monetary numeric displays such as item quantities, ratings, or dimension specifications

Exceptions:
- EXC-20-001: A third-party payment gateway or hosted checkout widget requires native unformatted values or renders its own self-contained monetary interface

## Rationale

- Observed imports across product description and cart modal components demonstrate an existing architectural convention of delegating price rendering to a single shared module.
- Encapsulating currency formatting logic within a dedicated component isolates internationalization and styling changes from surrounding business logic.
- Enforcing a shared presentation component prevents subtle visual bugs such as currency symbol omission or rounding discrepancies between catalog views and checkout previews.

## Consequences

Positive:
- Guarantees visual and mathematical consistency in currency presentation across all application screens.
- Reduces code duplication across presentation components by centralizing formatting rules.
- Simplifies future updates to currency formatting, multi-currency support, or internationalization standards to a single module.

Negative:
- Creates a coupling between individual presentation views and the shared Price component module contract.
- Introduces an additional component boundary for basic numeric display elements.

## Alternatives

- Ad-hoc inline string concatenation and formatting within each individual view component (rejected)
  Rejected because: Leads to duplicated formatting logic, divergent currency representations, and elevated maintenance overhead across shopping surfaces
  When valid: Rapid prototyping of disposable mockups with a single invariant currency
- Headless utility function returning formatted currency strings without a presentation component (rejected)
  Rejected because: Leaves wrapper markup and typographic styling duplicated across distinct consumer views
  When valid: Formatting prices for non-visual plain-text targets such as email subjects or system logs

## Risks

- Breaking interface changes in the shared Price component could impact multiple customer-facing screens simultaneously
  Mitigation: Establish strict automated regression test coverage for all supported property permutations before publishing updates
  Owner: Frontend Engineering Team
- Performance degradation if the shared component introduces excessive re-rendering in high-frequency list views
  Mitigation: Ensure the Price component remains lightweight, pure, and devoid of unnecessary internal state or side effects
  Owner: Frontend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure consumer components supply explicit currency codes alongside raw monetary values to avoid defaulting to incorrect locale assumptions.
- Verify that container styling passed into the shared component does not override accessibility labels or visual contrast requirements.

## Continuation Context


Verify commands:
- Discover and run the project repository linter and static analysis suite to identify unauthorized inline price formatting.
- Discover and execute the automated component test suite to validate Price component rendering across varied currencies and quantities.

Accept when:
- All automated component tests and static analysis verification scripts complete with zero errors.
- No consumer interface components implement independent currency symbol concatenation or ad-hoc number formatting.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline
- Verified by: Peer code review verification on all pull requests touching user interface presentation modules
- Violation handling: Pull requests containing inline price formatting or bypassing the shared component are blocked from merging until refactored.
- Exception process: Submit an architectural review request detailing third-party embed requirements or unresolvable layout constraints to the Frontend Architecture Lead for formal evaluation.