---
version: 2
name: 54aeac68-3abe-43aa-a222-e7adc4717bf8-react-promise-unwrapping-via-use-api-at-context-boundaries-consumer-discover-project-dependency-manifest-inspect
description: "React Promise Unwrapping via use API at Context Boundaries: Consumer Discover Project Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: 54aeac68-3abe-43aa-a222-e7adc4717bf8
    status: accepted
    candidate_group_id: 20960f3d-e58a-4d3f-bbd1-4049364fd937
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
---

# React Promise Unwrapping via use API at Context Boundaries: Consumer Discover Project Dependency Manifest Inspect

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/cart/**`

## Context

- Component hierarchies managing domain state require mechanisms to consume asynchronous resource promises without blocking synchronous renders.
- Adoption of the React use API enables client components to unwrap promise values directly within the component execution flow.
- Passing asynchronous promises through context establishes an architectural boundary where resolution is handled cooperatively with render boundaries.

## Problem Statement

Client state management often requires access to asynchronous data models, traditionally forcing components to maintain separate loading state flags and synchronization effects. A standardized mechanism is required to resolve context-provided promises synchronously during render using the adopted React runtime without introducing fragmented state handling.

## Decision

1. MUST: The consumer MUST discover the project dependency manifest and inspect the repository lock artifact to determine the exact locked version of React before utilizing asynchronous context unwrapping APIs.

## Policy Block

- MUST The consumer MUST discover the project dependency manifest and inspect the repository lock artifact to determine the exact locked version of React before utilizing asynchronous context unwrapping APIs.

In scope:
- Client components and context consumer hooks consuming asynchronous context promises.
- Context providers encapsulating asynchronous resource promises for subtree consumption.

Out of scope:
- Server-side rendering pipelines where promises are resolved ahead of rendering.
- Synchronous state contexts that do not expose or transport unresolved promises.

Exceptions:
- EXC-24-001: A consumer component requires custom background retry mechanisms before suspending the render tree.

## Rationale

- Utilizing the React use API allows direct promise unwrapping at context boundaries, integrating cleanly with component lifecycle mechanics.
- Eliminating manual resolution effects reduces boilerplate and prevents desynchronization between pending promises and local component state.
- Encapsulating asynchronous promises within context preserves clean separation between promise creation and consumption sites.

## Consequences

Positive:
- Unifies asynchronous context consumption under native React promise unwrapping semantics.
- Removes boilerplate state flags and effect-based synchronization from context consumer components.
- Enables declarative suspense boundary integration for pending context resources.

Negative:
- Requires enclosing boundary coordination to handle pending states and uncaught promise rejections.
- Increases cognitive overhead regarding execution constraints when unwrapping promises conditionally.

## Alternatives

- Manual effect-based state synchronization with local component state hooks (rejected)
  Rejected because: Introduces boilerplate state management, race condition hazards, and desynchronization between promise settlement and component rendering.
  When valid: Environments where the React runtime does not support native promise unwrapping APIs.
- Blocking server-side data resolution before context initialization (deferred)
  Rejected because: Prevents streaming and progressive rendering of client interactive context trees.
  When valid: Static context hierarchies where asynchronous client hydration is not required.

## Risks

- Triggering render suspensions without adequate boundary fallbacks resulting in unresponsive interface sections.
  Mitigation: Enforce boundary coverage around components consuming context promises via structural review.
  Owner: Frontend engineering team
- Uncaught promise rejections bubbling past context boundaries and crashing component subtrees.
  Mitigation: Ensure root and subtree error boundaries are deployed around all asynchronous context providers.
  Owner: Frontend engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure context values containing promises are memoized or stable across renders to prevent unnecessary re-unwrapping cycles.
- Verify that consumer components invoking the use API operate within active context provider scopes.

## Continuation Context


Verify commands:
- Discover the project test runner from the repository manifest and execute component test suites verifying context promise resolution.
- Discover the static analysis verification command from the repository configuration and validate hook usage rules across context consumer modules.

Accept when:
- All context consumer components unwrapping promises compile and pass static verification without hook rule violations.
- Component tests verify successful data unwrapping and proper fallback handling during pending promise states.

## Enforcement

- Verified by: Automated continuous integration pipeline running static analysis and test validation suites.
- Verified by: Peer code review verifying adherence to context promise unwrapping patterns.
- Violation handling: Pull request checks block merging if unapproved asynchronous context consumption patterns are detected.
- Violation handling: Violations identified during code review require refactoring to use standardized context unwrapping before approval.
- Exception process: Submit an architecture exception request detailing why standard promise unwrapping via context cannot be applied.
- Exception process: Obtain formal sign-off from the technical lead before merging non-standard context implementations.