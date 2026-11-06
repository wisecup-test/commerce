---
version: 2
name: e3bfc1d8-235e-4068-8fd2-6027de35a97e-shared-fragment-module-architecture-for-domain-queries-consumers-inspect-repository-lock-artifact-verify
description: "Shared Fragment Module Architecture for Domain Queries: Consumers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: e3bfc1d8-235e-4068-8fd2-6027de35a97e
    status: accepted
    candidate_group_id: d4161197-2aed-4594-b6a6-f649ed8252e5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Shared Fragment Module Architecture for Domain Queries: Consumers Inspect Repository Lock Artifact Verify

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `lib/shopify/queries/**`

## Context

- Domain data fetching requires consistent entity shapes across multiple query modules such as collection and product queries.
- Without modular fragment composition, entity field selections are repeatedly written across independent query files, leading to divergent schemas and high maintenance overhead.
- Centralizing common entity shapes into dedicated fragment modules allows query modules to compose standardized data contracts cleanly.

## Problem Statement

Maintaining independent, duplicative entity field definitions across multiple data access queries introduces schema drift, increases maintenance burden, and complicates upstream schema updates. An architectural convention is required to modularize and reuse entity field projections across all domain query modules.

## Decision

1. MUST: Consumers MUST inspect the repository lock artifact to verify the exact resolved version of any supporting libraries before implementing or modifying query execution infrastructure.

## Policy Block

- MUST Consumers MUST inspect the repository lock artifact to verify the exact resolved version of any supporting libraries before implementing or modifying query execution infrastructure.

In scope:
- Data queries requiring domain entity field selections that recur across multiple query endpoints.
- Shared data access modules defining reusable entity representations.

Out of scope:
- One-off queries with bespoke entity projections that share no overlap with domain entities.
- Transport-level network protocol implementations and raw client connection handlers.

## Rationale

- Reusing centralized fragment modules prevents schema drift across disparate query definitions that consume the same underlying entities.
- Composing queries from modular fragments reduces duplication and ensures maintenance updates propagate simultaneously to all dependent queries.
- Modular fragments isolate domain entity field selections, enabling domain query modules to remain focused on query orchestration and parameters.

## Consequences

Positive:
- Eliminates redundant entity field definitions across collection, product, and related query structures.
- Enforces consistent data contracts across query modules, lowering the risk of divergent schema representations.
- Simplifies maintenance by centralizing entity modifications into dedicated fragment modules.

Negative:
- Introduces inter-module dependency coupling between query modules and shared fragment modules.
- May result in minor over-fetching if individual queries require only a small subset of fields provided by a shared fragment.

## Alternatives

- Inline entity field definitions directly within each domain query module (rejected)
  Rejected because: Causes widespread duplication of entity field selections and introduces schema drift when entity structures evolve.
  When valid: Valid only for isolated, single-use queries where entity shapes are never reused elsewhere.
- Single monolithic schema file defining all queries and entity structures in one module (rejected)
  Rejected because: Creates merge bottlenecks, degrades maintainability, and breaks domain separation between distinct data access concerns.
  When valid: Valid only in small prototypes or micro-applications with negligible data access complexity.

## Risks

- Unintended over-fetching if shared fragments accumulate fields not required by all referencing queries.
  Mitigation: Decompose large fragments into granular, cohesive sub-fragments based on domain access patterns.
  Owner: Engineering Team
- Breaking changes to a shared fragment propagating unexpectedly to multiple downstream queries.
  Mitigation: Ensure comprehensive test validation and automated verification across all query modules before modifying shared fragments.
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
- Decompose shared entity representations into granular sub-fragments when differing query contexts require distinct subsets of fields.
- Organize fragment modules by domain entity concerns adjacent to corresponding query modules within the data access architecture.

## Continuation Context


Verify commands:
- Find the project verification script for module import analysis and execute it to verify that query modules reference shared fragment modules rather than inline definitions.
- Discover and run the project type-checking test suite to confirm query modules integrate fragment definitions cleanly without syntax or import errors.
- Query the version control history to verify that modifications to shared entity shapes update the corresponding fragment module rather than individual query definitions.

Accept when:
- All data query definitions compose shared fragment modules for common entity representations without inline field duplication.
- Automated static analysis and type checks pass across all query and fragment modules.
- Dependency resolution confirms all fragment imports resolve cleanly within the project architecture.

## Enforcement

- Verified by: Automated static analysis and code review checks during continuous integration.
- Verified by: Dependency boundary auditing to verify that query modules consume centralized fragment modules.
- Violation handling: Pull requests containing inline duplicates of existing fragment definitions are blocked until refactored into shared fragments.
- Violation handling: Unregistered query field structures are flagged for architectural review.
- Exception process: Architectural review submission detailing the performance or structural rationale for an isolated query projection.