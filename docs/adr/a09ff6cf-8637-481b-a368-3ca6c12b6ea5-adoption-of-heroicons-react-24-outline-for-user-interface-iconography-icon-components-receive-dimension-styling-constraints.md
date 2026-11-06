---
version: 2
name: a09ff6cf-8637-481b-a368-3ca6c12b6ea5-adoption-of-heroicons-react-24-outline-for-user-interface-iconography-icon-components-receive-dimension-styling-constraints
description: "Adoption of @heroicons/react/24/outline for User Interface Iconography: Icon Components Receive Dimension Styling Constraints"
metadata:
  actual:
    adr_id: a09ff6cf-8637-481b-a368-3ca6c12b6ea5
    status: accepted
    candidate_group_id: 79132c3d-c9da-4982-ba2a-7e35341d6aea
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - components/layout/**
      - components/product/**
---

# Adoption of @heroicons/react/24/outline for User Interface Iconography: Icon Components Receive Dimension Styling Constraints

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/cart/**`
- `components/layout/**`
- `components/product/**`

## Context

- User interface components across shopping cart controls, navigation menus, and product displays require uniform visual iconography for interactive actions.
- Previous implementations or unstandardized approaches risk inconsistent visual stroke weights, mismatched dimensions, and duplicated inline SVG markup.
- The codebase consolidates iconography onto the twenty-four pixel outline variant provided by the @heroicons/react/24/outline module across client-rendered components.

## Problem Statement

Ad-hoc inclusion of inline vector graphics or heterogeneous icon collections creates visual inconsistency, inflates bundle size with redundant SVG definitions, and complicates systematic theme adjustments across user interface controls.

## Decision

1. SHOULD: Icon components SHOULD receive dimension and styling constraints through standard style property classes rather than hardcoded vector attributes.

## Policy Block

- SHOULD Icon components SHOULD receive dimension and styling constraints through standard style property classes rather than hardcoded vector attributes.

In scope:
- All user interface components displaying decorative or action-associated iconography in navigation, search, cart, and product views.

Out of scope:
- Branded third-party payment vendor logos, corporate trademarks, and complex multi-color domain illustrations.

## Rationale

- Evidence demonstrates ninety percent consistency in icon imports originating from @heroicons/react/24/outline across navigation, search, and cart workflows.
- Standardizing on a single twenty-four pixel outline set guarantees consistent visual stroke weight, proportion, and responsive styling throughout the component hierarchy.
- Importing pre-compiled React icon components maintains tree-shaking capabilities and minimizes repetitive SVG boilerplate.

## Consequences

Positive:
- Ensures cohesive visual language and consistent stroke geometry across all user interface interactions.
- Eliminates repetitive inline SVG markup, improving readability and maintainability in component templates.
- Leverages compiler tree-shaking to include only the explicitly referenced icon assets in client bundles.

Negative:
- Restricts visual iconography to the glyphs provided by the designated outline set unless custom assets are authored.
- Introduces an external module dependency that must be tracked and locked across environments.

## Alternatives

- Inline SVG elements authored directly within each component template (rejected)
  Rejected because: Creates visual inconsistency across features, inflates component file sizes, and prevents unified theme updates
  When valid: When rendering bespoke graphic marks that have no reusable glyph equivalent in standard collections
- Solid variant icon collection from the same design family (rejected)
  Rejected because: Violates the visual balance established by the outline style pervasive in existing user interface controls
  When valid: When high-contrast emphasis or filled status badges are explicitly mandated by visual specifications

## Risks

- Missing glyphs for specialized domain concepts within the adopted icon collection
  Mitigation: Encapsulate domain-specific custom icons within dedicated shared components styled to match outline stroke conventions
  Owner: engineering team
- Tree-shaking failure causing entire icon library bundles to be shipped to clients
  Mitigation: Enforce explicit named imports directly from the specific module subpath during automated checks
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
- Locate the component hierarchy where user interface controls reside and import required icons via named bindings from the specified outline path.
- Apply sizing and color tokens uniformly via component property bindings to preserve outline stroke clarity across screen resolutions.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute it to validate that all icon imports resolve against the locked dependency version.
- Execute the repository linting and static analysis tasks to ensure no unauthorized icon libraries or unapproved inline vector graphics have been introduced.

Accept when:
- All interactive user interface controls render iconography sourced exclusively from @heroicons/react/24/outline without static analysis errors.
- The repository verification suite passes with zero unresolved module imports.

## Enforcement

- Verified by: Automated static analysis and code review checks during the pull request validation process.
- Verified by: Repository dependency audit tools verifying import paths against approved dependency lists.
- Violation handling: Pull requests containing unauthorized icon imports or raw inline SVG icons without an approved exception will be blocked by automated review policies.
- Exception process: Submit an architectural review request outlining the missing glyph requirement and propose a conformant SVG implementation matching stroke weight standards.