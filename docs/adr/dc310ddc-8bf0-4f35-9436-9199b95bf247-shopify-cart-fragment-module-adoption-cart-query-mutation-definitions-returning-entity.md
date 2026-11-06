---
version: 2
name: dc310ddc-8bf0-4f35-9436-9199b95bf247-shopify-cart-fragment-module-adoption-cart-query-mutation-definitions-returning-entity
description: "Shopify Cart Fragment Module Adoption: Cart Query Mutation Definitions Returning Entity"
metadata:
  actual:
    adr_id: dc310ddc-8bf0-4f35-9436-9199b95bf247
    status: accepted
    candidate_group_id: 7bc01f66-13c9-48f1-bd0f-3b89df4574ac
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Cart Fragment Module Adoption: Cart Query Mutation Definitions Returning Entity

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `lib/shopify/**`

## Context

- E-commerce cart workflows require synchronized data structures between read operations and state-modifying write operations.
- Declaring entity attributes independently across multiple operations causes schema drift between query responses and mutation responses.
- The data access architecture separates cart queries and cart mutations into distinct operational modules while demanding uniform entity representation.
- A shared cart fragment module provides a centralized specification of cart fields reusable across operational boundaries.

## Problem Statement

Cart read queries and state-modifying mutations risk returning divergent data shapes when entity field selections are declared independently, producing client-side cache inconsistencies and redundant maintenance across query and mutation boundaries.

## Decision

1. MUST: All cart query and mutation definitions returning cart entity payloads MUST compose their response selections using the shared cart fragment module.

## Policy Block

- MUST All cart query and mutation definitions returning cart entity payloads MUST compose their response selections using the shared cart fragment module.

In scope:
- Data retrieval queries that return cart domain entity representations
- State-modifying mutations that return updated cart domain entity representations

Out of scope:
- Operations targeting domain entities outside the cart lifecycle
- Operational status checks and existence checks that do not return cart entity bodies

## Rationale

- Composing cart queries and mutations through a shared fragment module enforces field parity across read and write operations.
- A single fragment definition simplifies maintenance by propagating cart schema enhancements across all consuming operations.
- Static analysis demonstrates deliberate reuse of the cart fragment module across both query and mutation layers.

## Consequences

Positive:
- Guarantees schema parity between query reads and mutation updates for cart domain entities.
- Centralizes cart entity attribute maintenance into a single authoritative definition.
- Simplifies client cache normalization by maintaining uniform entity shapes across network responses.

Negative:
- Increases response payload size for operations requiring only a small subset of cart entity attributes.
- Modifications to the shared fragment impact all dependent queries and mutations simultaneously.

## Alternatives

- Independent inline field selections declared separately within each query and mutation (rejected)
  Rejected because: Permits schema drift between read operations and write operations, creating inconsistent client-side entity representations.
  When valid: Valid in isolated disposable prototypes where client cache normalization and payload consistency are unnecessary.
- Dedicated per-operation fragment definitions for individual queries and mutations (rejected)
  Rejected because: Multiplies maintenance overhead and prevents uniform cache updates across operations acting on the same domain entity.
  When valid: Valid when strict network payload constraints require custom minimal field selections per distinct view.

## Risks

- Over-fetching data when a specific operation requires only minimal status attributes rather than the full cart entity payload.
  Mitigation: Profile response payload sizes and introduce nested sub-fragments if over-fetching impacts critical latency boundaries.
  Owner: engineering team
- Breaking changes introduced to the shared cart fragment causing simultaneous failures across all dependent queries and mutations.
  Mitigation: Validate fragment schema compatibility and compile all queries against the remote schema during continuous integration.
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
- Compose all newly defined cart query and mutation operations by importing and interpolating the shared cart fragment module definition.
- Verify that document interpolation syntax adheres to the schema validation requirements of the project data client.

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis and query validation suite to ensure all cart query and mutation documents compile with the shared cart fragment.
- Discover and run the project's type-checking suite to confirm that generated query and mutation response types resolve against the shared cart fragment definition.

Accept when:
- All cart query and mutation definitions successfully compile and incorporate the shared cart fragment module without validation errors.
- Static type checking passes across all cart query and mutation modules with zero schema divergence.

## Enforcement

- Verified by: Automated static analysis and schema validation suites executed in continuous integration pipelines
- Verified by: Peer code reviews for all pull requests modifying cart query or mutation modules
- Violation handling: Pull requests containing cart queries or mutations with ad-hoc field selections that bypass the shared cart fragment module are blocked from merging.
- Exception process: Exceptions require architectural review documenting specific payload budget constraints that necessitate an isolated query definition.