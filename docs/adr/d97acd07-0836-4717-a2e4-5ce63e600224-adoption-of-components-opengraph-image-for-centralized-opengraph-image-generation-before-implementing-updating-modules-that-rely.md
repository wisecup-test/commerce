---
version: 2
name: d97acd07-0836-4717-a2e4-5ce63e600224-adoption-of-components-opengraph-image-for-centralized-opengraph-image-generation-before-implementing-updating-modules-that-rely
description: "Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Before Implementing Updating Modules That Rely"
metadata:
  actual:
    adr_id: d97acd07-0836-4717-a2e4-5ce63e600224
    status: accepted
    candidate_group_id: f7137deb-eaa3-44c3-b6f2-14b4fdfa2721
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Before Implementing Updating Modules That Rely

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `app/**`

## Context

- Social share previews require dynamic image generation across diverse route segments including top-level pages, dynamic content pages, and collection search routes.
- Directly composing visual layout and graphic styling inside each individual route image handler duplicates layout logic and risks visual inconsistency across endpoints.
- The application establishes a centralized module to encapsulate common OpenGraph image styling, layout structure, and graphic asset placement while accepting dynamic parameters.

## Problem Statement

Dynamic OpenGraph images must be generated across multiple route hierarchies while maintaining uniform branding, typography, and responsive presentation layouts. Implementing independent image layout and styling markup in each route handler introduces visual fragmentation, code duplication, and heightened maintenance cost when design updates occur.

## Decision

1. MUST: Before implementing or updating modules that rely on versioned rendering or graphic generation dependencies, developers MUST inspect the repository lock artifact and verify exact resolved dependency versions against current documentation.

## Policy Block

- MUST Before implementing or updating modules that rely on versioned rendering or graphic generation dependencies, developers MUST inspect the repository lock artifact and verify exact resolved dependency versions against current documentation.

In scope:
- All route-level social metadata and OpenGraph image generation handlers across root and dynamic route segments.

Out of scope:
- Static raster images served as immutable public assets.
- Client-rendered UI components outside server-side social metadata generation pipelines.

## Rationale

- Evidence demonstrates consistent delegation of image rendering to `components/opengraph-image` across all route-level OpenGraph image generation endpoints.
- Decoupling data fetching from layout rendering allows route handlers to focus on parameter extraction and service calls while preserving shared visual presentation contracts.
- Centralizing presentation in a dedicated module simplifies cross-platform social card layout maintenance and brand asset governance.

## Consequences

Positive:
- Enforces consistent branding, typographic hierarchy, and visual design across all dynamically generated social preview cards.
- Isolates data fetching and route parameter parsing from visual presentation and image compositing logic.
- Reduces code duplication across social media image generation endpoints.

Negative:
- Couples all route-level OpenGraph image generation handlers to the layout contract and dependencies of the shared component module.
- Changes to the presentation layout in the shared component require regression testing across all consuming route variants.

## Alternatives

- Decentralized route-specific OpenGraph image markup (rejected)
  Rejected because: Duplicating markup, styling, and asset references directly within each route handler creates visual drift and increases maintenance overhead across routes.
  When valid: Valid only when a route requires a fundamentally distinct visual paradigm or media format that the shared component cannot accommodate.
- Static pre-rendered image assets for all routes (rejected)
  Rejected because: Static images cannot reflect dynamic entity state such as collection names, dynamic page titles, or localized branding.
  When valid: Valid when all route content is completely static and known at compilation time.

## Risks

- A regression in the shared image presentation module could degrade social preview generation across all routes simultaneously.
  Mitigation: Maintain comprehensive automated snapshot and rendering tests for the shared component across diverse title lengths and parameter configurations.
  Owner: Frontend Engineering Team
- Overly rigid component layout constraints may prompt developers to bypass the shared component for complex routes.
  Mitigation: Define clean, flexible optional parameter interfaces on the shared component to accommodate contextual route needs without bypassing the module.
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
- Inspect existing route image handlers to identify the parameter signature and property contracts expected by the shared image presentation component.
- Ensure dynamic entity resolution and error handling occur within route scopes before invoking the image generation presentation module.

## Continuation Context


Verify commands:
- Discover the repository build and static analysis commands from package manifests and run lint checks across all route image handlers.
- Discover and execute the project test runner to verify that social image generation handlers resolve and execute without errors.

Accept when:
- Static analysis and lint checks complete with zero errors across all OpenGraph image route handlers.
- All OpenGraph image route endpoints successfully resolve and render preview images using the shared presentation module without interface or type mismatches.

## Enforcement

- Verified by: Automated static analysis and code review checks during continuous integration.
- Verified by: Component integration tests verifying social image route rendering contracts.
- Violation handling: Pull requests introducing decentralized image layout markup or bypassing the shared presentation module fail automated review gates.
- Violation handling: Violations must be refactored to consume the shared image component before branch merge.
- Exception process: Submit an architectural review request documenting why the shared image component cannot satisfy the visual or technical requirements of the route handler.
- Exception process: Obtain approval from frontend engineering leads before introducing bespoke image generation logic.