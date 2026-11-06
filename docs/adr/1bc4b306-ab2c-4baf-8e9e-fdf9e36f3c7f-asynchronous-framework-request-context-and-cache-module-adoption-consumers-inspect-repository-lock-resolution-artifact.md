---
version: 2
name: 1bc4b306-ab2c-4baf-8e9e-fdf9e36f3c7f-asynchronous-framework-request-context-and-cache-module-adoption-consumers-inspect-repository-lock-resolution-artifact
description: "Asynchronous Framework Request Context and Cache Module Adoption: Consumers Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 1bc4b306-ab2c-4baf-8e9e-fdf9e36f3c7f
    status: accepted
    candidate_group_id: e608c980-ba4d-4597-8582-d6f9665f77c8
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - lib/shopify/**
---

# Asynchronous Framework Request Context and Cache Module Adoption: Consumers Inspect Repository Lock Resolution Artifact

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/cart/**`
- `lib/shopify/**`

## Context

- Server-side execution contexts require consistent patterns for accessing incoming request metadata and maintaining stateful session identifiers such as cart references across server actions and external API adapters.
- The application relies on asynchronous request utilities to access cookies and headers directly within server-side components and integration boundaries rather than threading request objects.
- Cache lifecycle and tag revalidation mechanisms require coordinated invocation alongside mutating actions and external webhook reception.

## Problem Statement

Handling request context, session identifiers, and cache invalidation across distributed server actions and external integration boundaries risks inconsistent state and fragile prop threading if request primitives are accessed inconsistently.

## Decision

1. MUST: Consumers MUST inspect the repository lock or resolution artifact to determine the exact resolved framework version before implementing asynchronous header or cookie calls.

## Policy Block

- MUST Consumers MUST inspect the repository lock or resolution artifact to determine the exact resolved framework version before implementing asynchronous header or cookie calls.

In scope:
- All server actions, data fetching routines, and external API client adapters running in server execution contexts requiring request header or cookie access.
- Modules handling server-side cache revalidation and webhook payload processing.

Out of scope:
- Client-side interactive components running exclusively in browser runtimes.
- Pure computation utilities and isolated formatting functions with no external or HTTP context dependencies.

## Rationale

- Evidence confirms consistent adoption of framework-provided asynchronous header and cookie access alongside core caching modules in multiple integration modules.
- Using native asynchronous primitives prevents leaking request objects across internal functional boundaries and preserves server component encapsulation.
- Standardizing on framework cache revalidation hooks ensures downstream state mutations accurately synchronize with server-rendered data.

## Consequences

Positive:
- Decouples data access and client boundary operations from explicit request parameter passing by reading cookies and headers at point of use.
- Centralizes cache revalidation controls around native framework caching primitives, ensuring reliable tag invalidation.
- Provides safe server-side retrieval of transport tokens and stateful identifiers without exposing them to client-side scripts.

Negative:
- Asynchronous access semantics require awaiting headers and cookie utilities, creating potential runtime execution delays if called redundantly.
- Tight coupling of server actions and external client integrations to framework-specific runtime primitives limits portability across different hosting environments.
- Improperly awaited asynchronous primitives will result in runtime exceptions or unresolved Promise handles.

## Alternatives

- Direct request object passing through component and server action props (rejected)
  Rejected because: Leads to prop drilling of request context into internal logic and prevents decoupled access to cookie state across service boundaries.
  When valid: When operating within a purely synchronous HTTP middleware or custom server routing layer outside framework conventions.
- Client-side cookie and cache management via browser APIs (rejected)
  Rejected because: Exposes sensitive identifiers and prevents unified server-side cache revalidation and cart synchronization.
  When valid: When building static single-page client applications without server rendering capabilities.

## Risks

- Failure to await asynchronous request primitives can cause runtime type errors or silent Promise leaks in server execution contexts.
  Mitigation: Enforce strict static analysis rules and type checking ensuring all asynchronous framework primitives are awaited before property access.
  Owner: engineering team
- Unexpected failure during cookie mutation or cache invalidation can leave user session state out of sync with external services.
  Mitigation: Wrap cookie updates and cache revalidations within structured error boundaries and fallback handlers.
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
- Encapsulate cookie reads and writes within dedicated data access helpers to avoid scattering asynchronous resolution across presentation components.
- Combine header inspections for verification secrets and webhook topics within gateway or boundary modules before invoking domain services.

## Continuation Context


Verify commands:
- Discover and execute the repository type-check script to verify asynchronous return types on all header and cookie access invocations.
- Discover and execute the repository linter script to detect un-awaited framework primitives or unauthorized direct request manipulations.
- Discover and run the project test suite covering server actions and integration boundaries to validate state and revalidation handling.

Accept when:
- Server action and data access modules interact with HTTP request headers and cookies strictly through asynchronous helper invocations.
- Cache revalidation and tag invalidation are handled via framework-level cache utilities rather than manual cache resets.
- Static verification checks pass without unhandled Promise rejections or direct header/cookie manipulation without awaiting the framework primitives.

## Enforcement

- Verified by: Automated linting rules checking for proper asynchronous usage of framework cookie and header access utilities.
- Verified by: Pull request peer review validating server action signatures and cache boundary isolation.
- Violation handling: Pull requests containing un-awaited framework header or cookie calls will be blocked by automated checks.
- Violation handling: Violations identified during code review will require refactoring to use framework server access primitives before merging.
- Exception process: Exceptions require review by the architecture team with documented justification demonstrating why asynchronous framework utilities cannot satisfy the use case.
- Exception process: Temporary exemptions must include a scheduled migration ticket to align with standard server primitives.