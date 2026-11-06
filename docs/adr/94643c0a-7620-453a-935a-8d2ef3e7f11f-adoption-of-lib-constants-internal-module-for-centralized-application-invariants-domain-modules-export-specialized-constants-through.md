---
version: 2
name: 94643c0a-7620-453a-935a-8d2ef3e7f11f-adoption-of-lib-constants-internal-module-for-centralized-application-invariants-domain-modules-export-specialized-constants-through
description: "Adoption of lib/constants Internal Module for Centralized Application Invariants: Domain Modules Export Specialized Constants Through"
metadata:
  actual:
    adr_id: 94643c0a-7620-453a-935a-8d2ef3e7f11f
    status: accepted
    candidate_group_id: b6447dca-b165-4cdd-80f5-5a5fa0295df5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/product/**
      - app/search/**
      - components/cart/**
      - components/layout/search/filter/**
      - lib/shopify/**
---

# Adoption of lib/constants Internal Module for Centralized Application Invariants: Domain Modules Export Specialized Constants Through

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `app/product/**`
- `app/search/**`
- `components/cart/**`
- `components/layout/search/filter/**`
- `lib/shopify/**`

## Context

- Application functionality spans multiple architectural layers including client rendering components, server layout wrappers, server mutation actions, and external client abstractions.
- Scattering invariant values, configuration strings, and domain constants across individual files leads to value drift, magic strings, and duplicated definitions.
- A centralized internal module provides a consistent location for shared invariants accessible across both server and client execution boundaries.

## Problem Statement

Without a standardized internal module for cross-cutting constants, domain values and configuration parameters become duplicated across route handlers, presentation components, mutation actions, and integration clients. This duplication introduces drift, complicates configuration refactoring, and weakens structural boundaries between application layers.

## Decision

1. MAY: Domain modules MAY re-export specialized constants through scoped namespace wrappers when domain encapsulation is explicitly required.

## Policy Block

- MAY Domain modules MAY re-export specialized constants through scoped namespace wrappers when domain encapsulation is explicitly required.

In scope:
- Application source modules defining or consuming cross-cutting constants, configuration parameters, or shared domain invariants.
- Components, route handlers, server actions, and service client definitions requiring access to invariant values.

Out of scope:
- Local transient variables confined strictly to a single private function scope.
- Dynamic runtime configuration values injected exclusively via environment variables or external service requests.

## Rationale

- Centralizing domain constants in lib/constants guarantees single-source-of-truth semantics across presentation and data access layers.
- Static module imports enable straightforward dependency tracking and refactoring across diverse execution contexts.
- Eliminating inline magic strings reduces divergence between UI routing parameters and server action payload handling.

## Consequences

Positive:
- Guarantees single-source-of-truth semantics for shared parameters and domain values across client and server execution contexts.
- Eliminates magic strings and divergent configuration across UI route filters, server actions, and external client integrations.
- Simplifies refactoring and auditability of application-wide invariant values.

Negative:
- Central module coupling across diverse functional domains may increase bundle footprint if module tree-shaking is not properly configured.
- Modifications to core shared constants trigger widespread rebuilds and revalidation across consumer modules.

## Alternatives

- Distributed local constant definitions within individual feature modules (rejected)
  Rejected because: Causes value drift, duplicate declarations, and inconsistent parameter handling across client components and server actions.
  When valid: When a constant is strictly private to a single isolated component with no external dependencies or cross-module coupling.
- Runtime configuration service or context provider for all invariant values (rejected)
  Rejected because: Introduces unnecessary runtime overhead and component context ceremony for values that are statically invariant at compile time.
  When valid: When configuration values must be dynamically mutated at runtime or retrieved from an external service per user session.

## Risks

- Accumulation of unrelated domain constants into a monolithic module leading to tight coupling.
  Mitigation: Enforce domain-based export grouping and separate concern boundaries within the module.
  Owner: engineering team
- Accidental inclusion of sensitive runtime secrets or server-only values in constants consumed by client components.
  Mitigation: Strictly limit module contents to public invariants and enforce static checks preventing secret exposure.
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
- Group exports within the module logically by domain concern to facilitate maintainability and clear consumption semantics.
- Ensure that exported constants remain strictly immutable primitive or readonly structures to prevent accidental mutation across module boundaries.

## Continuation Context


Verify commands:
- Discover the project verification script defined in the repository configuration and execute the static analysis checks.
- Discover the test runner from the project configuration and execute the test suite to validate module resolution and constant imports.

Accept when:
- Static analysis validates that all shared constants and invariants are imported from the centralized lib/constants module without unresolved references.
- All test suites pass with no duplicate invariant definitions across presentation components, route handlers, and server actions.

## Enforcement

- Verified by: Automated static analysis checks verifying module imports and detecting duplicated constant literals.
- Verified by: Peer code review during pull request evaluation.
- Violation handling: Automated verification fails if duplicated invariant literals or unauthorized local overrides are detected.
- Violation handling: Code review blocks merging until shared values are consolidated into the designated internal module.
- Exception process: Architectural review and documented approval justifying local isolation for strictly component-specific private values.