---
version: 2
name: b6a211a5-0966-4c62-ac6e-65c052933127-shopify-client-module-lib-shopify-commerce-catalog-operations-product-search-queries
description: "Shopify Client Module (lib/shopify): Commerce Catalog Operations Product Search Queries"
metadata:
  actual:
    adr_id: b6a211a5-0966-4c62-ac6e-65c052933127
    status: accepted
    candidate_group_id: a61abcae-9d2e-4506-ac32-4166f16a7b12
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
---

# Shopify Client Module (lib/shopify): Commerce Catalog Operations Product Search Queries

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `app/search/**`

## Context

- Search and collection route components require coordinated access to e-commerce catalog data, sorting definitions, and product collections.
- Directly invoking third-party e-commerce APIs within presentation routes leads to scattered credentials, fragmented query logic, and inconsistent data transformations.
- Consolidating e-commerce data access into a dedicated domain module establishes a reliable boundary between external service contracts and application components.

## Problem Statement

Search and collection views across the application require product and catalog data from an external e-commerce platform. Without an architectural boundary governing how this data is retrieved, individual routes risk coupling themselves directly to third-party endpoints, introducing duplicate query logic, disparate authentication handling, and fragile data mappings across route hierarchies.

## Decision

1. MUST: All e-commerce catalog operations, product search queries, and collection retrievals MUST be executed through the dedicated lib/shopify client module rather than direct external API calls from presentation routes.

## Policy Block

- MUST All e-commerce catalog operations, product search queries, and collection retrievals MUST be executed through the dedicated lib/shopify client module rather than direct external API calls from presentation routes.

In scope:
- Route handlers, page components, and search views requiring e-commerce product and collection data.
- Internal modules managing catalog queries, collection filtering, and product sorting.

Out of scope:
- Static marketing layouts and utility components that do not interact with catalog or store APIs.
- Client-side visual presentation components that receive pre-fetched data via props.

## Rationale

- Multiple routing endpoints demonstrate identical dependency structures relying on lib/shopify for product retrieval and sorting resolution.
- Isolating integration logic in a dedicated module limits external API breaking changes to a single surface area within the codebase.
- Consistent data models emitted by the module allow reusable layout and grid components to operate on stable property contracts.

## Consequences

Positive:
- Decouples user interface rendering and route handlers from third-party API endpoint specifics and authentication mechanisms.
- Provides a single point of enforcement for caching, request memoization, error handling, and payload normalization.
- Ensures consistent product sorting, collection retrieval, and catalog data structures across all search and listing views.

Negative:
- Centralized module maintenance overhead increases as new e-commerce endpoints and domain features are added.
- UI routes depend entirely on module contracts, requiring coordinated updates when domain entity shapes evolve.

## Alternatives

- Direct route-level external store API invocations via inline network fetch calls (rejected)
  Rejected because: Causes fragmented authentication handling, duplicate schema parsing, and high coupling between UI routes and external API specifications.
  When valid: Rapid single-file prototypes where no architectural reuse or centralized domain modeling is necessary.
- Generic third-party multi-tenant e-commerce abstraction library (rejected)
  Rejected because: Introduces unnecessary structural complexity and leaky abstractions when the platform targets a specific provider ecosystem.
  When valid: Multi-vendor architectures requiring dynamic switching between distinct e-commerce backend providers at runtime.

## Risks

- Upstream provider API contract updates or deprecations could disrupt dependent route components.
  Mitigation: Establish thorough integration test suites covering the module's query and response handling against external provider updates.
  Owner: Engineering Team
- Centralizing all catalog queries through a single module could introduce latency bottlenecks if data fetching is not appropriately memoized.
  Mitigation: Implement response caching, request memoization, and selective field queries within the centralized client module.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Implement query functions inside the centralized module to return strongly typed domain objects rather than raw external API payloads.
- Ensure sorting parameters received by route components are validated against established constant collections prior to initiating catalog requests.

## Continuation Context


Verify commands:
- Inspect the repository configuration to locate and execute the project's static analysis script to verify module import boundary compliance.
- Discover and run the project's automated test runner from the root manifest to validate catalog retrieval and sorting query behavior.

Accept when:
- All automated test suites covering catalog data fetching and search query resolution pass without regressions.
- Static analysis and module boundary checks confirm no direct external store API requests bypass the centralized module.

## Enforcement

- Verified by: Continuous integration static analysis rules checking import boundaries to prevent unauthorized external network requests in presentation layers.
- Verified by: Automated integration test suites validating module contracts against resolved dependency versions.
- Verified by: Peer code review verifying adherence to centralized data client module patterns.
- Violation handling: Static analysis failures immediately block pull request merges when presentation components bypass the integration module.
- Violation handling: Violations flagged during review require refactoring unauthorized external requests into the designated client module.
- Exception process: Submit an architectural review request detailing external store capabilities that cannot be accommodated within the centralized module boundary.
- Exception process: Document justification and obtain approval from the core engineering team prior to deploying isolated network calls.