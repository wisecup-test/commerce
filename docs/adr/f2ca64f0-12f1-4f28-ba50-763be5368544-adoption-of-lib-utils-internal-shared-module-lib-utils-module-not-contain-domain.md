---
version: 2
name: f2ca64f0-12f1-4f28-ba50-763be5368544-adoption-of-lib-utils-internal-shared-module-lib-utils-module-not-contain-domain
description: "Adoption of lib/utils Internal Shared Module: Lib Utils Module Not Contain Domain"
metadata:
  actual:
    adr_id: f2ca64f0-12f1-4f28-ba50-763be5368544
    status: accepted
    candidate_group_id: 1fedad58-d6b8-493a-bef9-5cef8b98908b
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
      - components/layout/search/filter/**
      - lib/shopify/**
---

# Adoption of lib/utils Internal Shared Module: Lib Utils Module Not Contain Domain

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `app/**`
- `components/layout/search/filter/**`
- `lib/shopify/**`

## Context

- Multiple independent subsystems across routing, external service integration, and client presentation require common utility routines.
- Uncoordinated implementation of utility functions across separate application layers leads to code divergence and inconsistent behavior.
- A shared internal module provides a single source of truth for cross-cutting utility helpers across server and client execution boundaries.

## Problem Statement

Without a unified shared utility module, divergent utility implementations proliferate across server-side route generation, external client integration boundaries, and client-side presentation components, increasing maintenance overhead and the risk of behavioral inconsistencies.

## Decision

1. MUST_NOT: The lib/utils module MUST NOT contain domain-specific business logic or client-specific state management that creates circular dependencies with consuming layers.

## Policy Block

- MUST_NOT The lib/utils module MUST NOT contain domain-specific business logic or client-specific state management that creates circular dependencies with consuming layers.

In scope:
- Application route handlers, domain integration clients, and user interface components requiring shared utility routines.

Out of scope:
- Domain-specific business logic, persistent data models, and isolated single-use component algorithms.

## Rationale

- Evidence demonstrates consistent consumption of lib/utils across metadata generation handlers, domain integration adaptors, and presentation filter components.
- Centralizing shared helpers within lib/utils enforces consistency in data formatting, sanitization, and parameter manipulation across disparate application tiers.
- Isolating reusable utility procedures into a dedicated module decouples presentation and data retrieval layers from low-level operational routines.

## Consequences

Positive:
- Eliminates duplicate helper implementations across server and client boundaries.
- Standardizes cross-cutting utility behaviors across metadata generation, external integrations, and user interface components.
- Simplifies maintenance by providing a single location for utility function updates and unit test coverage.

Negative:
- Creates a centralized dependency that can cause widespread regressions if utility signatures or behaviors change.
- Increases coupling between otherwise independent subsystems that rely on the shared module.

## Alternatives

- Inlining localized utility functions directly within consuming route handlers and components (rejected)
  Rejected because: Inlining leads to behavioral divergence, code duplication, and redundant test overhead across modules.
  When valid: When a utility function is strictly proprietary to a single component and has zero reusability across other layers.
- Publishing utility functions to an external package registry (rejected)
  Rejected because: Publishing externally introduces release pipeline overhead and version latency for internal application utilities.
  When valid: When utility functions must be shared across independent repository boundaries and distinct product teams.

## Risks

- Accumulation of domain-specific logic inside the utility module causing high coupling
  Mitigation: Enforce strict separation between domain logic and generic utility helpers through architecture reviews and linting rules.
  Owner: engineering team
- Server-only or client-only API leakage into shared utility functions breaking multi-environment execution
  Mitigation: Ensure all utility functions in the shared module are isomorphic and independent of environment-specific globals.
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
- Audit proposed utility additions to verify they are domain-agnostic and broadly reusable across multiple architectural layers.
- Provide comprehensive unit test coverage for each exported utility function to protect against regressions across consuming modules.

## Continuation Context


Verify commands:
- Discover and execute the project static analysis and type verification script to ensure all references to lib/utils satisfy type contracts.
- Discover and execute the project test runner to validate that utility functions pass all unit tests.

Accept when:
- All consuming modules resolve imports from lib/utils with zero compilation or static analysis diagnostics.
- All automated unit tests covering functions in lib/utils pass without regression.

## Enforcement

- Verified by: Automated static analysis checks in continuous integration pipelines
- Verified by: Peer code review during pull request evaluation
- Violation handling: Pull requests introducing duplicate utility logic or circular dependencies are blocked until resolved.
- Exception process: Exceptions require documented approval from the technical architecture lead detailing why localized utilities are strictly necessary.