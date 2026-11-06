---
version: 2
name: dba1a9ba-6a66-4ab7-91a1-433b83e54f4b-adoption-of-components-prose-module-for-rich-text-typography-page-templates-view-components-not-apply
description: "Adoption of components/prose Module for Rich Text Typography: Page Templates View Components Not Apply"
metadata:
  actual:
    adr_id: dba1a9ba-6a66-4ab7-91a1-433b83e54f4b
    status: accepted
    candidate_group_id: ef1ad579-dd4f-481b-8561-23724c669a3d
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
      - components/product/**
---

# Adoption of components/prose Module for Rich Text Typography: Page Templates View Components Not Apply

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `app/**`
- `components/product/**`

## Context

- Multiple user interface templates across route handlers and presentation components render rich text and descriptive body content.
- Directly applying typography classes in individual view components leads to inconsistent text styling, duplicated layout declarations, and visual drift across different application views.
- Code analysis reveals standardized consumption of the components/prose internal module across dynamic page templates and product presentation layers.

## Problem Statement

Uncoordinated styling of rich text, descriptions, and editorial body content across distinct route templates and presentation components causes typographic drift and maintenance overhead. The application requires a centralized module to encapsulate rich text typography rules while allowing parent views to manage exterior layout.

## Decision

1. MUST_NOT: Page templates and view components MUST_NOT apply direct typography overrides to internal HTML elements rendered within the prose container hierarchy.

## Policy Block

- MUST_NOT Page templates and view components MUST_NOT apply direct typography overrides to internal HTML elements rendered within the prose container hierarchy.

In scope:
- Rendering dynamic multi-paragraph HTML or markdown text content in route templates and view components.
- Displaying product descriptions and long-form editorial copy requiring standardized typography.

Out of scope:
- Rendering isolated single-line text elements, form labels, or UI button labels.
- Non-visual utility functions, data fetching modules, and API route handlers.

## Rationale

- Centralizing typography rules within the components/prose module enforces uniform typographic hierarchy across all rich text views without duplicating styling rules across route boundaries.
- Encapsulating prose formatting within an internal component decouples data-fetching templates from low-level typographic presentation mechanics.
- Reusing the prose component across both dynamic route templates and product views ensures that markdown or rich content received from external services renders with identical visual hierarchy.

## Consequences

Positive:
- Ensures consistent typographic hierarchy and visual cadence across all views that render long-form or dynamic rich text.
- Eliminates duplicate typographic style definitions in individual route and presentation components.
- Simplifies maintenance by concentrating typography style updates into a single internal module.

Negative:
- Creates a centralized dependency where modifications to the prose module affect every view rendering rich text.
- Reduces layout flexibility for individual views that require non-standard typography treatments.

## Alternatives

- Authoring inline typographic utility classes directly within each page template and presentation component (rejected)
  Rejected because: Causes severe styling duplication, leads to visual inconsistency between routes, and increases maintenance cost when typography design rules change
  When valid: Valid only in small prototypes or single-view applications where global typography consistency is unnecessary
- Relying strictly on unstyled native HTML elements without a specialized prose wrapper component (rejected)
  Rejected because: Produces unstyled or browser-default typography that conflicts with design system visual standards
  When valid: Valid in headless API services or plain-text environments where visual presentation is not required

## Risks

- Changes to centralized prose styling could unintentionally alter the layout of existing dynamic pages or product descriptions
  Mitigation: Perform visual regression verification across all views consuming the prose module prior to merging styling updates
  Owner: Frontend engineering team
- Developers might bypass the prose module and write bespoke classes for new rich text views
  Mitigation: Enforce prose module usage through automated lint rules and pull request reviews
  Owner: Frontend engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Discover the project component directory and import the prose component into any template requiring multi-element rich text rendering.
- Pass rich text markup or children to the prose component while applying outer positioning, margins, and borders to the parent container.
- Ensure that styling frameworks or typography plugins referenced by the prose module are resolved through the project lock artifact.

## Continuation Context


Verify commands:
- Discover the project test runner script from the package manifest and execute the test suite to verify component rendering integrity.
- Discover the repository linter script from the build configuration and execute it to verify compliance with module import conventions.
- Discover the project build verification command from the workspace manifest and execute a production build to ensure static type consistency.

Accept when:
- The project test suite passes with all rich text rendering components mounting the prose module without errors.
- The project linter passes with zero violations regarding prohibited inline typography formatting in presentation templates.
- The project build verification command completes successfully with no type resolution or import errors.

## Enforcement

- Verified by: Continuous integration pipelines executing repository linter and test validation scripts.
- Verified by: Peer code review verifying that rich text display delegates to the prose module.
- Violation handling: Pull requests containing inline typography overrides on rich text containers will be blocked until refactored to use the prose module.
- Violation handling: Automated lint failures will reject builds that bypass standardized presentation components.
- Exception process: Exceptions require documented justification detailing why standard prose typography cannot satisfy the view constraints, along with engineering team approval.