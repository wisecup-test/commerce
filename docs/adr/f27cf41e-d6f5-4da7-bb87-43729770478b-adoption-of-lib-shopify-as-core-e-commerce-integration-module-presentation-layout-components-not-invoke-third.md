---
version: 2
name: f27cf41e-d6f5-4da7-bb87-43729770478b-adoption-of-lib-shopify-as-core-e-commerce-integration-module-presentation-layout-components-not-invoke-third
description: "Adoption of lib/shopify as Core E-Commerce Integration Module: Presentation Layout Components Not Invoke Third"
metadata:
  actual:
    adr_id: f27cf41e-d6f5-4da7-bb87-43729770478b
    status: accepted
    candidate_group_id: 4351d544-a887-4734-b4b3-6c3c9ad9ed15
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/**
---

# Adoption of lib/shopify as Core E-Commerce Integration Module: Presentation Layout Components Not Invoke Third

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/layout/**`

## Context

- Layout navigation components require e-commerce domain entities, including navigation menus, cart state, and product identifiers, to construct global user interface elements.
- Direct invocations of external e-commerce platform APIs from presentation components introduce duplication, inconsistent error handling, and leaky transport concerns.
- Static analysis identifies consistent imports of the internal client module lib/shopify and its shared contracts lib/shopify/types across structural layout components.

## Problem Statement

Presentation components that fetch external e-commerce data independently risk fragmenting domain logic, duplicating type definitions, and complicating external platform migrations.

## Decision

1. MUST_NOT: Presentation layout components MUST NOT invoke third-party e-commerce endpoints directly or bypass the lib/shopify module boundary.

## Policy Block

- MUST_NOT Presentation layout components MUST NOT invoke third-party e-commerce endpoints directly or bypass the lib/shopify module boundary.

In scope:
- User interface components and structural layout elements requiring access to store resources or operational domain data.
- Data fetching and domain mutation operations interacting with the external e-commerce platform.

Out of scope:
- Pure utility functions that manipulate primitives without domain context.
- Presentational components that receive all domain data via props and make no direct data requests.

## Rationale

- Centralizing domain operations within lib/shopify establishes an explicit architectural boundary between external commerce service APIs and internal user interface components.
- Reusing entity contracts from lib/shopify/types prevents schema drift across disjoint layout elements.
- Evidence across layout components demonstrates existing adoption of lib/shopify as the authoritative access layer for commerce entities.

## Consequences

Positive:
- Isolates UI rendering from external platform API contract changes and transport details.
- Provides unified type contracts across headers, footers, and auxiliary layout components.
- Simplifies auditing and caching of e-commerce data operations within a single integration surface.

Negative:
- Couples layout components to the internal function signatures and maintenance lifecycle of lib/shopify.
- Requires all new e-commerce capabilities to be implemented and reviewed within the central module before UI consumption.

## Alternatives

- Direct API invocations from individual presentation components (rejected)
  Rejected because: Scattering raw fetch calls across components creates maintenance overhead, inconsistent authentication handling, and duplicated data schemas.
  When valid: One-off prototyping experiments where long-term maintainability is explicitly deprioritized.
- Third-party generic headless client embedded across components (rejected)
  Rejected because: Exposes vendor-specific client conventions directly to layout components without an internal domain adaptation boundary.
  When valid: Projects with no custom domain logic or data normalization requirements.

## Risks

- Changes to external e-commerce platform schemas can break consumers across multiple layout components if module exports fall out of synchronization.
  Mitigation: Validate lib/shopify exports and shared type contracts against platform schema updates during automated verification.
  Owner: engineering team
- Performance bottlenecks if all layout components request overlapping data independently through the module.
  Mitigation: Implement request deduplication and caching mechanisms inside lib/shopify function exports.
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
- Expose typed async query and mutation functions from lib/shopify to encapsulate all external payload transformation and endpoint addressing.
- Maintain entity definitions in lib/shopify/types as the single source of truth for e-commerce domain models across layout components.

## Continuation Context


Verify commands:
- Discover and execute the repository type checking script to verify that all layout components consume lib/shopify and lib/shopify/types without compilation errors.
- Discover and run the architectural linter configured in the project to verify that no direct external e-commerce API calls exist outside of lib/shopify.

Accept when:
- Static type analysis passes with zero type errors across all layout component imports.
- Boundary linting confirms all e-commerce operations originate exclusively from lib/shopify.

## Enforcement

- Verified by: Automated continuous integration checks executing type verification and import boundary rules.
- Verified by: Peer code review for pull requests modifying layout components or e-commerce integration code.
- Violation handling: Pull requests introducing direct external API calls or duplicate domain types are blocked until refactored to consume lib/shopify.
- Exception process: Architecture review and documented justification approved by the engineering team prior to bypassing the module.