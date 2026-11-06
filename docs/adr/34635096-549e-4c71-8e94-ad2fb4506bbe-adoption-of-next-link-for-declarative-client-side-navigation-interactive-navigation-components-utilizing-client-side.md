---
version: 2
name: 34635096-549e-4c71-8e94-ad2fb4506bbe-adoption-of-next-link-for-declarative-client-side-navigation-interactive-navigation-components-utilizing-client-side
description: "Adoption of next/link for Declarative Client-Side Navigation: Interactive Navigation Components Utilizing Client Side"
metadata:
  actual:
    adr_id: 34635096-549e-4c71-8e94-ad2fb4506bbe
    status: accepted
    candidate_group_id: 2ab43c5c-4665-4c51-bd45-48eecd759009
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of next/link for Declarative Client-Side Navigation: Interactive Navigation Components Utilizing Client Side

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/**`

## Context

- Frontend layout and presentation components across multiple directories require uniform internal navigation mechanisms to provide seamless page transitions without triggering full document reloads.
- Static analysis of eight interface components spanning navigation bars, mobile menus, search filters, footers, carousels, and grid displays identifies consistent adoption of next/link for declarative internal routing.
- Standardizing internal navigation on next/link enables viewport-based background prefetching and unified client-side route transitions across both server-rendered and client-rendered component trees.

## Problem Statement

Unmanaged hyperlink rendering across disparate layout and catalog components creates inconsistent user experiences, risks unnecessary full-page browser document reloads for internal transitions, and bypasses route prefetching optimizations provided by the application framework.

## Decision

1. SHOULD: Interactive navigation components utilizing client-side hooks alongside next/link SHOULD isolate client interaction boundaries using explicit client rendering directives while keeping non-interactive presentation shells as server-rendered components where applicable.

## Policy Block

- SHOULD Interactive navigation components utilizing client-side hooks alongside next/link SHOULD isolate client interaction boundaries using explicit client rendering directives while keeping non-interactive presentation shells as server-rendered components where applicable.

In scope:
- User interface layout and presentation components that render internal application links, navigation menus, product listings, search filters, and carousels.
- Components operating within both server and client rendering boundaries requiring client-side page transitions.

Out of scope:
- External hyperlinks directed to third-party domains outside the application host.
- Non-visual background data fetching routines and server-only API endpoints.

Exceptions:
- EXC-20-001: A third-party authentication redirect or external federated service requires a full browser document navigation instead of a client-side transition.

## Rationale

- Adopting next/link as the universal internal link primitive across layout and presentation components ensures that all route transitions leverage client-side virtual routing and automated background prefetching.
- Consistently applying next/link across both client components and server components maintains route state and search parameters predictably across the user interface.
- Centralizing navigation behavior on a single declarative primitive eliminates bespoke anchor event interception handlers across presentation widgets.

## Consequences

Positive:
- Enables single-page application route transitions with automatic background prefetching for internal links within the viewport.
- Ensures consistent URL state and query parameter handling across navigation menus, product grids, and search filters.
- Reduces network latency and page transition overhead compared to full document reloads.

Negative:
- Couples presentation components to framework-specific routing modules and their component property contracts.
- Introduces client bundle overhead when navigation components require client hydration boundaries.
- Requires developers to distinguish between internal route paths and external destinations to avoid routing errors.

## Alternatives

- Standard HTML anchor elements without framework routing primitives (rejected)
  Rejected because: Native anchor elements cause full browser document reloads on internal route navigation, losing client-side application state and forfeiting automated route prefetching.
  When valid: Valid only for external hyperlinks pointing outside the application domain or downloadable file assets.
- Imperative programmatic navigation triggered via click event handlers (rejected)
  Rejected because: Imperative routing complicates standard browser behaviors such as opening links in new tabs, impairs search engine crawlability, and requires client component event handling for static links.
  When valid: Valid when route transitions must occur conditionally after asynchronous side effects or programmatic form submissions.

## Risks

- Accidental use of next/link for external URLs leading to unexpected client router failures or invalid route resolution.
  Mitigation: Enforce linting and static analysis rules that validate destination targets or use an external URL detection abstraction.
  Owner: Frontend Engineering Team
- Excessive background prefetching on dense product grid displays saturating client network connections.
  Mitigation: Configure selective prefetch attributes on high-density listing components where viewport prefetching is non-essential.
  Owner: Frontend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When constructing complex dynamic route destinations with multiple query parameters, assemble parameters using standard query object abstractions before passing the serialized path string to next/link.
- Ensure presentation components intended to operate on the server do not introduce client event handlers directly onto next/link unless wrapped within designated client component boundaries.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting validation scripts to confirm all internal navigational links utilize next/link.
- Discover and run the project automated test suite to ensure navigational components render valid destination route attributes without runtime errors.

Accept when:
- All internal navigation elements across layout and presentation components resolve to next/link instances without unmanaged internal anchor elements.
- Static analysis and test suites execute with zero navigation-related lint errors or broken route reference failures.

## Enforcement

- Verified by: Automated continuous integration lint checks enforcing import rules for internal navigation.
- Verified by: Peer code review for all pull requests introducing new layout or presentation link components.
- Violation handling: Pull requests containing native anchor tags for internal application navigation are blocked until refactored to use next/link.
- Exception process: Developers submit an architecture exception request detailing why standard client-side routing is incompatible with the specific link destination.