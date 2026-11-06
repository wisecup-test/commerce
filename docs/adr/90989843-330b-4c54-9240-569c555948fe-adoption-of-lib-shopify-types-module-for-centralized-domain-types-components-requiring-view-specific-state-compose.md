---
version: 2
name: 90989843-330b-4c54-9240-569c555948fe-adoption-of-lib-shopify-types-module-for-centralized-domain-types-components-requiring-view-specific-state-compose
description: "Adoption of lib/shopify/types Module for Centralized Domain Types: Components Requiring View Specific State Compose"
metadata:
  actual:
    adr_id: 90989843-330b-4c54-9240-569c555948fe
    status: accepted
    candidate_group_id: e4172a9d-5b98-40c4-9425-311efc04f707
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - components/grid/**
      - components/layout/**
      - components/product/**
---

# Adoption of lib/shopify/types Module for Centralized Domain Types: Components Requiring View Specific State Compose

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/cart/**`
- `components/grid/**`
- `components/layout/**`
- `components/product/**`

## Context

- Components responsible for catalog navigation, product displays, variant selections, and cart interactions require consistent data structures representing domain entities.
- Direct ad-hoc type declarations or local interface definitions within individual presentation components lead to schema drift and mismatched property assumptions.
- The internal module lib/shopify/types provides shared structural interfaces for commerce entities across both client and layout layers.
- Multiple user interface components and client state contexts consume domain entities that must align with upstream commerce schemas.

## Problem Statement

Without a standardized domain contract module, individual user interface components, cart providers, and navigation layouts risk duplicating or diverging in their data contracts for commerce entities, leading to runtime data mapping errors, property naming inconsistencies, and maintenance overhead across UI boundaries.

## Decision

1. MAY: Components requiring view-specific UI state MAY compose or extend base domain types from the lib/shopify/types module provided that underlying domain entity contracts remain unmodified.

## Policy Block

- MAY Components requiring view-specific UI state MAY compose or extend base domain types from the lib/shopify/types module provided that underlying domain entity contracts remain unmodified.

In scope:
- Frontend presentation components, layout structures, and client state contexts interacting with commerce domain entities.
- Components handling product catalogs, product variant selections, and shopping cart operations.

Out of scope:
- Standalone generic presentation components that do not consume commerce data structures.
- Low-level utility functions operating strictly on primitive data types without domain semantics.

Exceptions:
- EXC-20-001: Third-party vendor widgets or isolated legacy scripts require unique unstructured data payloads that cannot map to standard commerce entity interfaces.

## Rationale

- The pattern is evidenced across ten independent files spanning cart management, product presentation, and navigation layout modules that uniformly import domain definitions from lib/shopify/types.
- Centralizing domain interfaces into a single-source-of-truth module eliminates schema drift between independent components and prevents contract mismatches between client contexts and presentation elements.
- Enforcing shared domain contracts simplifies refactoring and upstream data model upgrades by consolidating interface changes into a single module.

## Consequences

Positive:
- Guarantees contract uniformity and compile-time type safety across all components handling commerce entities.
- Eliminates redundant local type declarations and reduces the surface area for interface drift.
- Improves developer productivity by providing standard autocompletion and structural documentation across client modules.

Negative:
- Couples presentation and client components to central domain type definitions, requiring coordinated updates when upstream schemas evolve.
- Requires developers to map ad-hoc or external payloads into standardized domain types prior to consumption in UI components.

## Alternatives

- Component-local interface declarations for domain entities (rejected)
  Rejected because: Creates redundant definitions across components, introduces schema drift, and increases maintenance cost when domain structures evolve.
  When valid: Valid only in isolated prototypes or decoupled micro-frontends with no shared codebase.
- Unconstrained generic objects or untyped data passing (rejected)
  Rejected because: Eliminates compile-time contract safety, increases runtime errors during property access, and degrades editor code navigation.
  When valid: Valid only when handling dynamic, arbitrary payloads where structures cannot be known at build time.

## Risks

- Upstream schema changes may break multiple component interfaces simultaneously due to centralized coupling.
  Mitigation: Version domain type contracts carefully and use typecheck verification in continuous integration before publishing schema modifications.
  Owner: Frontend Engineering Team
- Over-specialization of domain types may lead developers to bypass the central module for complex view-specific states.
  Mitigation: Establish clear patterns for composing and extending base domain types with view-specific properties.
  Owner: Frontend Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Discover the project centralized domain type definitions by inspecting the shared domain module before declaring new types for commerce entities.
- Ensure all newly introduced UI components interacting with catalog or cart state import domain entities directly from the central module rather than recreating local interfaces.

## Continuation Context


Verify commands:
- Discover and run the project static typecheck command to verify all component implementations conform to the centralized domain type contracts without type diagnostics.
- Discover and run the project linter command to ensure no prohibited local duplications of domain types or unconstrained object usage exist.

Accept when:
- The project typecheck process completes with zero type errors across all modules importing lib/shopify/types.
- All components consuming commerce domain structures explicitly reference types from the central domain module.

## Enforcement

- Verified by: Automated static type checking and linting executed during continuous integration workflows.
- Verified by: Peer code review verifying that new commerce-facing components utilize the shared domain type module.
- Violation handling: Pull requests introducing local duplicate domain types or failing typecheck verification are blocked from merging until resolved.
- Exception process: Exceptions must be submitted via an architecture review request detailing the necessity for distinct contracts and specifying a migration plan.