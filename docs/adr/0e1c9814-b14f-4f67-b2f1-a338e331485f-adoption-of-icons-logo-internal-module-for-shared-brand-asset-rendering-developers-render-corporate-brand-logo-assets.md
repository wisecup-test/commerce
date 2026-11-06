---
version: 2
name: 0e1c9814-b14f-4f67-b2f1-a338e331485f-adoption-of-icons-logo-internal-module-for-shared-brand-asset-rendering-developers-render-corporate-brand-logo-assets
description: "Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Developers Render Corporate Brand Logo Assets"
metadata:
  actual:
    adr_id: 0e1c9814-b14f-4f67-b2f1-a338e331485f
    status: accepted
    candidate_group_id: 3b7fb158-9916-4e65-972c-4185a31a0e1f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of ./icons/logo Internal Module for Shared Brand Asset Rendering: Developers Render Corporate Brand Logo Assets

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/**`

## Context

- The application requires consistent visual branding across client-facing user interface components and dynamic social sharing preview images.
- Vector brand assets defined multiple times create divergence in visual rendering, aspect ratios, and color styling across view surfaces.
- Static analysis detects shared consumption of the internal ./icons/logo component module across distinct presentation and metadata generation components.

## Problem Statement

Scattering raw SVG paths or ad-hoc image definitions of the corporate logo across different user interface components and metadata image templates leads to visual drift, maintenance friction, and styling inconsistencies when brand assets evolve.

## Decision

1. MUST: Developers MUST render corporate and brand logo assets through the shared ./icons/logo internal module across all user interface components and dynamic visual metadata generators.

## Policy Block

- MUST Developers MUST render corporate and brand logo assets through the shared ./icons/logo internal module across all user interface components and dynamic visual metadata generators.

In scope:
- Rendering corporate or brand logo assets in client-side or server-rendered user interface components.
- Generating dynamic visual metadata artifacts, including social media preview images.

Out of scope:
- Third-party partner marks or sponsor logos that do not represent primary brand identity.
- Generic utility icons or non-brand decorative glyphs.

Exceptions:
- EXC-20-001: A specialized rendering environment cannot execute module imports or component runtimes and requires a pre-rendered static raster fallback.

## Rationale

- Centralizing brand imagery in ./icons/logo establishes a single source of truth for visual identity across standard application views and dynamic OpenGraph image generators.
- Eliminating duplicated vector path definitions across components reduces maintenance overhead when brand assets update.
- Reusing the component across both interactive UI surfaces and server-driven image generation guarantees brand visual consistency across different consumer touchpoints.

## Consequences

Positive:
- Provides a single canonical source of truth for brand vector data and visual identity.
- Ensures consistent logo presentation across both interactive UI views and dynamic social sharing preview images.
- Streamlines updates to brand iconography without sweeping modifications across disparate component trees.

Negative:
- Couples dynamic metadata image generation pipelines to internal component module contracts.
- Modifications to component properties or internal dependencies can propagate regression risks to both UI layouts and metadata image generation.

## Alternatives

- Inline vector markup embedded directly within each consuming presentation and metadata component. (rejected)
  Rejected because: Causes duplicate SVG definitions, drift between social previews and UI headers, and high maintenance costs when updating brand geometry.
  When valid: Single-use prototypes or standalone static pages with no shared component architecture.
- External static raster or vector asset files loaded via image reference tags. (rejected)
  Rejected because: Prevents dynamic styling, runtime class injection, and embedded SVG styling necessary for consistent rendering across server and client boundaries.
  When valid: Static non-interactive landing pages hosting fixed-dimension external media.

## Risks

- Server-side image generation environments may fail if ./icons/logo introduces client-only runtime APIs or unsupported styling constructs.
  Mitigation: Keep ./icons/logo pure and presentational without client-only state, hooks, or browser-specific globals.
  Owner: Frontend engineering team
- Modifications to the shared logo component may inadvertently alter dimensions or layouts across diverse consuming contexts.
  Mitigation: Execute automated visual regression and component test suites across consuming views prior to merging updates.
  Owner: UI engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Import ./icons/logo directly into brand-rendering containers and supply configuration properties for dimensions and class names.
- Verify that all properties passed into ./icons/logo maintain compatibility with both browser runtime layouts and server-side visual generation contexts.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute component testing across all visual modules importing ./icons/logo.
- Execute the repository visual regression and linting suites discovered in the project automation pipeline to validate brand rendering integrity.

Accept when:
- All user interface components and metadata generators render brand assets exclusively through the shared ./icons/logo module with zero inline SVG duplicates.
- Component test suites and visual regression checks pass without layout shifts or rendering failures across both interactive and metadata generation contexts.

## Enforcement

- Verified by: Automated static analysis and lint rules forbidding duplicated brand SVG paths in application components.
- Verified by: Peer code reviews and architectural checks on pull requests modifying presentation and metadata layers.
- Verified by: Continuous integration test passes verifying component rendering and visual regression outputs.
- Violation handling: Automated continuous integration checks reject pull requests containing direct SVG duplicates of the brand logo.
- Violation handling: Engineers must refactor violating components to consume the shared ./icons/logo module before merge approval.
- Exception process: Submit an architectural review request citing the specific rendering constraint that precludes shared module import.
- Exception process: Obtain explicit approval from the frontend architecture lead with documented fallback rationale.