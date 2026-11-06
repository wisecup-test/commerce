---
version: 2
name: 42372ac7-6a00-4af5-81eb-1f9941793c08-centralized-commerce-data-access-and-domain-module-via-lib-shopify-presentation-components-application-route-handlers-not
description: "Centralized Commerce Data Access and Domain Module via lib/shopify: Presentation Components Application Route Handlers Not"
metadata:
  actual:
    adr_id: 42372ac7-6a00-4af5-81eb-1f9941793c08
    status: accepted
    candidate_group_id: 200a08a3-c441-4571-bdcc-aa8c14b01867
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
      - components/**
---

# Centralized Commerce Data Access and Domain Module via lib/shopify: Presentation Components Application Route Handlers Not

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `app/**`
- `components/**`

## Context

- Applications interacting with external commerce backends require consistent querying, type definitions, and cache invalidation protocols across multiple route handlers, page components, and user interaction handlers.
- Dispersing raw external client calls or query payloads across individual views and mutation handlers risks contract divergence, duplicated transport logic, and fragile error handling.
- Centralizing commerce operations into a dedicated internal domain module provides a single boundary for domain models, query execution, and revalidation logic across all application entry points.
- Static analysis across the codebase demonstrates widespread consumption of this internal module for product retrieval, collection browsing, cart mutations, sitemap generation, and dynamic image rendering.

## Problem Statement

Directly coupling user interface components, server actions, dynamic image generators, and route handlers to external commerce APIs introduces redundant transport logic, schema mismatches, and fragmented state management. The application requires a unified architectural boundary that encapsulates all commerce operations, domain type definitions, and backend communication protocols behind a standardized internal module.

## Decision

1. MUST_NOT: Presentation components and application route handlers MUST NOT define ad-hoc external commerce network requests or vendor-specific query payloads outside the dedicated module boundary.

## Policy Block

- MUST_NOT Presentation components and application route handlers MUST NOT define ad-hoc external commerce network requests or vendor-specific query payloads outside the dedicated module boundary.

In scope:
- Server components, route handlers, server actions, and metadata generators querying or mutating e-commerce domain resources.
- Components rendering catalog items, navigation links, cart state, or dynamic promotional displays.

Out of scope:
- Pure utility functions and presentational layout components that receive domain data solely via component properties.
- Generic platform routing and foundational font or styling configuration modules.

## Rationale

- Static analysis confirms 14 distinct files across routing, actions, layout, and presentation layers uniformly depend on the centralized commerce module, demonstrating established architectural encapsulation.
- Encapsulating commerce client operations behind an internal module isolates the application from external service contract alterations and centralizes response parsing and error handling.
- Providing unified type definitions and domain query functions through a shared module prevents duplicate query logic and guarantees consistent cache tagging across server and client surfaces.

## Consequences

Positive:
- Decouples visual presentation components and route handlers from external platform network details and schema idiosyncrasies.
- Standardizes cache revalidation, error logging, and session identification across mutation and data retrieval flows.
- Provides compile-time type safety across the entire application via shared domain type contracts.

Negative:
- Creates a centralized dependency bottleneck where modifications to core module interfaces can impact multiple consumer surfaces.
- Requires intermediate mapping layers between external backend responses and internal domain representations.

## Alternatives

- Direct external API querying from individual components and route handlers (rejected)
  Rejected because: Scatters query strings, endpoint configuration, and authentication details across UI and route files, increasing maintenance burden and schema drift risk.
  When valid: Valid only in single-file prototypes or throwaway exploratory scripts where long-term architectural maintenance is not required.
- Multiple fragmented client libraries segmented by individual routes or features (rejected)
  Rejected because: Introduces divergent client configurations, redundant network connection pooling, and inconsistent caching behaviors across the application.
  When valid: Valid when distinct domains are governed by completely independent sub-teams operating in separate micro-frontends or distributed microservices.

## Risks

- External service contract modifications breaking the core module and causing widespread application failures.
  Mitigation: Implement strict schema validation and automated integration tests against mock provider responses within the continuous verification pipeline.
  Owner: Engineering team
- Performance degradation if heavy client-side bundles inadvertently import server-only module utilities.
  Mitigation: Maintain strict boundary separation between server-only domain operations and client-safe data contracts.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Expose domain operations through modular asynchronous functions that handle authentication, network requests, and response validation internally.
- Export dedicated domain types alongside operation methods to ensure consumers maintain strict static typing across route handlers and views.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and type-checking scripts to verify that all commerce imports reference the centralized module.
- Discover and execute the automated test suite to ensure commerce queries, mutations, and revalidation workflows pass without errors.
- Discover and run the dependency audit script to confirm resolution artifacts match project manifest constraints.

Accept when:
- Static analysis confirms zero direct external commerce API network requests across presentation components and route handlers.
- All unit and integration test suites validating the centralized commerce module pass without errors.
- Dependency resolution checks verify that external commerce runtime dependencies match authoritative lock file definitions.

## Enforcement

- Verified by: Automated continuous integration checks executing static analysis, type checking, and test suites.
- Verified by: Peer code review validating adherence to domain module architectural boundaries.
- Violation handling: Pull requests containing direct external commerce endpoint calls or bypassing the centralized module will be blocked.
- Violation handling: Violating code must be refactored to consume the centralized domain interfaces prior to merge approval.
- Exception process: Developers must submit an architectural exception request detailing technical blockers to the engineering leadership team.
- Exception process: Approved temporary exceptions must include a deprecation milestone and a refactoring task tracked in the issue tracker.