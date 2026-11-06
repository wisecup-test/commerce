---
version: 2
name: bfe35ab5-c388-4290-970c-973ab21c16ff-console-error-logging-for-server-operations-before-adopting-integrating-any-versioned-external
description: "Console Error Logging for Server Operations: Before Adopting Integrating Any Versioned External"
metadata:
  actual:
    adr_id: bfe35ab5-c388-4290-970c-973ab21c16ff
    status: accepted
    candidate_group_id: 593f3998-bd25-40e8-b4c5-ef6bf73ae5f1
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - lib/shopify/**
---

# Console Error Logging for Server Operations: Before Adopting Integrating Any Versioned External

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `components/cart/**`
- `lib/shopify/**`

## Context

- Server actions and data integration modules execute boundary operations requiring diagnostics when exceptions or validation failures occur.
- The codebase relies on runtime standard error reporting to communicate validation failures and caught action exceptions without importing external logging frameworks.

## Problem Statement

Server-side operations and external integration endpoints require consistent error visibility when runtime exceptions or authorization validation failures occur, without introducing unverified third-party logging overhead.

## Decision

1. MUST: Before adopting or integrating any versioned external logging library, the consumer MUST inspect the repository lock artifact and verify the exact resolved version according to the mandatory lock-version grounding policy.

## Policy Block

- MUST Before adopting or integrating any versioned external logging library, the consumer MUST inspect the repository lock artifact and verify the exact resolved version according to the mandatory lock-version grounding policy.

In scope:
- Server-side actions, data layer integration boundaries, and webhook revalidation handlers encountering caught exceptions or invalid request states.

Out of scope:
- Client-side UI rendering components, static view templates, and pure utility transformation functions.

## Rationale

- Evidence confirms consistent use of runtime console.error across distinct server boundaries for error reporting.
- Native runtime logging satisfies baseline operational observability without incurring external dependency management overhead.

## Consequences

Positive:
- Zero external runtime dependencies required for operational error logging.
- Immediate execution visibility across server-side action handlers and validation boundaries.

Negative:
- Unstructured log output complicates parsing and automated alerting in high-volume production environments.
- Lack of unified log level management prevents dynamic runtime suppression of non-critical operational messages.

## Alternatives

- Third-party structured logging library adoption (rejected)
  Rejected because: Adds external package dependencies and configuration overhead for basic operational error reporting without dedicated aggregation infrastructure in the observed evidence.
  When valid: Valid when distributed tracing, metric correlation, and centralized structured log indexers require structured JSON schemas.
- Silent exception suppression without diagnostic recording (rejected)
  Rejected because: Obscures runtime failures and security invalidations, making production debugging and auditing impossible.
  When valid: Valid only for benign, fully recovered control-flow fallbacks where diagnostic tracking is explicitly redundant.

## Risks

- Potential exposure of sensitive tokens or headers in standard error output streams.
  Mitigation: Sanitize all logged variables and error context objects prior to error emission to prevent credential leakage.
  Owner: Engineering team
- Inability to parse unstructured text logs effectively in automated monitoring systems.
  Mitigation: Define standard error message prefixes and structure error arguments to assist downstream stream collectors.
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
- Log emissions must clearly delineate operational error causes without exposing sensitive authorization tokens or request secrets.
- Error instances should be passed directly to preserve native runtime stack trace information.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts to verify error handling compliance across server modules.
- Discover and execute the project test runner to validate that error branches emit diagnostic logs as expected.

Accept when:
- Static analysis confirms all designated server-side error paths and validation guard branches route error diagnostics to the standard error stream.
- Automated testing verifies that failed transactional operations emit diagnostic records without unhandled promise rejections.

## Enforcement

- Verified by: Automated static analysis checks in continuous integration pipelines.
- Verified by: Peer review during pull request evaluations of server-side error handlers.
- Violation handling: Pull requests with unlogged caught exceptions or non-compliant logging calls are blocked until corrected.
- Violation handling: Static analysis rule violations fail build steps automatically.
- Exception process: Submit an architectural review request outlining why a module requires structured output or silent fallback handling.
- Exception process: Document the approved logging variance in the module documentation prior to merging.