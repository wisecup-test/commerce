---
version: 2
name: 64972eb4-7498-4467-8beb-7a2545075583-adoption-of-shared-seo-fragment-module-for-query-composition-domain-query-definitions-not-declare-custom
description: "Adoption of Shared SEO Fragment Module for Query Composition: Domain Query Definitions Not Declare Custom"
metadata:
  actual:
    adr_id: 64972eb4-7498-4467-8beb-7a2545075583
    status: accepted
    candidate_group_id: 6c3b4e2b-88b7-4462-a4da-7a0f7e4a8746
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Adoption of Shared SEO Fragment Module for Query Composition: Domain Query Definitions Not Declare Custom

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `lib/shopify/queries/**`

## Context

- Multiple domain query definitions retrieve distinct entity models while requiring standardized search engine optimization metadata attributes.
- Directly inlining identical metadata attribute selections across separate query modules introduces schema drift and redundant maintenance overhead.
- A shared fragment module structure encapsulates common metadata query fields for cross-cutting composition into domain queries.

## Problem Statement

Defining duplicate search engine optimization field selections across independent entity queries risks structural divergence, inconsistent responses, and redundant schema maintenance when metadata models evolve.

## Decision

1. MUST_NOT: Domain query definitions MUST NOT declare custom or ad-hoc metadata selections that bypass the shared SEO fragment module.

## Policy Block

- MUST_NOT Domain query definitions MUST NOT declare custom or ad-hoc metadata selections that bypass the shared SEO fragment module.

In scope:
- Entity and collection query definitions requiring search engine optimization metadata selections.

Out of scope:
- Non-metadata query operations or utility modules that do not handle entity SEO attributes.

## Rationale

- Importing the shared SEO fragment module across distinct query definitions centralizes metadata attribute selections into a single maintainable unit.
- Reusing standardized fragment modules enforces uniform response payloads across page and collection queries.
- Modifications to search engine optimization attribute requirements propagate across all consuming queries from a single point of edit.

## Consequences

Positive:
- Centralizes metadata query definitions into a single reusable module across domain query definitions.
- Guarantees consistent data shapes for search engine optimization attributes across multiple entity retrieval points.
- Eliminates redundant query field declarations and mitigates schema divergence between distinct entity queries.

Negative:
- Couples disparate entity queries to a unified fragment schema, where schema expansion increases payload size for all importing queries.
- Requires cross-query coordination and regression verification whenever the shared fragment contract is modified.

## Alternatives

- Inlining search engine optimization field selections directly within individual query definitions (rejected)
  Rejected because: Introduces repetitive field selections, schema drift across endpoints, and elevated maintenance costs when metadata fields evolve
  When valid: Valid only in isolated one-off scripts or exploratory prototypes where shared contracts are unnecessary
- Constructing metadata query strings dynamically through runtime concatenation functions (rejected)
  Rejected because: Obscures static query structure, prevents compile-time query validation, and introduces unnecessary runtime overhead
  When valid: Valid when metadata field selection must vary dynamically based on runtime query parameters

## Risks

- Unintended query payload expansion if the shared fragment grows with fields not required by all consuming queries.
  Mitigation: Keep the shared SEO fragment module strictly scoped to essential common metadata fields to prevent unnecessary payload overhead.
  Owner: engineering team
- Breaking changes to the shared fragment schema immediately impact all importing query definitions.
  Mitigation: Execute query document validation and end-to-end query tests across all consuming modules prior to merging fragment alterations.
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
- Define domain query fragments as exported definitions within dedicated fragment modules and import them into query definition files.
- Compose parent queries by interpolating the imported fragment definition and referencing its spread within the selection set.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking and schema validation scripts to ensure all query definitions and fragment spreads type-check cleanly.
- Discover and run the project test suite to verify that query modules correctly construct and execute queries containing shared fragments.

Accept when:
- Static type checking and query schema validation pass with zero errors across all query definitions referencing the shared SEO fragment module.
- All repository test suites asserting query document construction and data retrieval succeed.

## Enforcement

- Verified by: Automated continuous integration pipelines executing repository validation and test suites.
- Verified by: Peer code review verifying that domain queries compose metadata fields via the shared SEO fragment module instead of inline selections.
- Violation handling: Pull requests containing duplicated inline metadata query selections are blocked until refactored to import the shared SEO fragment module.
- Exception process: Submit an architectural review request documenting why the domain query requires custom metadata fields that cannot be satisfied by the shared SEO fragment module.