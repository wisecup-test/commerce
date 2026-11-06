---
version: 2
name: d030e74c-6399-4d74-8c51-753adfbe5af1-shopify-storefront-api-client-encapsulation-internal-boundary-validate-inbound-webhook-revalidation
description: "Shopify Storefront API Client Encapsulation: Internal Boundary Validate Inbound Webhook Revalidation"
metadata:
  actual:
    adr_id: d030e74c-6399-4d74-8c51-753adfbe5af1
    status: accepted
    candidate_group_id: cdc033d6-461b-4495-949d-3f387bfbdc99
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Storefront API Client Encapsulation: Internal Boundary Validate Inbound Webhook Revalidation

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Applies to:
- `lib/shopify/**`

## Context

- External interactions with the Shopify Storefront API require authenticated GraphQL POST requests using store domain and storefront access token credentials.
- Consuming application workflows require retrieval of session attributes and webhook headers such as cart identifiers and topic markers to manage cache state and data invalidation.
- Exposing raw transport logic, header injection, and environment configuration across application features creates fragmentation and increases the surface area for credential leaks.

## Problem Statement

Directly invoking external commerce endpoints across application modules scatters transport configurations, header-based authentication, and webhook revalidation logic throughout the codebase, making credential handling inconsistent and complicating cache tag management.

## Decision

1. MUST: The internal boundary MUST validate inbound webhook revalidation secrets prior to triggering cache tag invalidation or data revalidation routines.

## Policy Block

- MUST The internal boundary MUST validate inbound webhook revalidation secrets prior to triggering cache tag invalidation or data revalidation routines.

In scope:
- Outbound HTTP requests and GraphQL queries targeting the Shopify Storefront API.
- Inbound webhook verification and cache revalidation handlers associated with Shopify topics.

Out of scope:
- Internal cross-module function calls that do not communicate with external commerce endpoints.
- Static client-side assets and presentation components with no direct integration requirements.

## Rationale

- Isolating external API calls within a single internal client boundary prevents scattering external authentication tokens and request formatting across features.
- Encapsulating header extraction and secret verification protects cache revalidation endpoints against unauthorized cache purging.
- Centralized POST request serialization ensures predictable query and variable payloads for external GraphQL execution.

## Consequences

Positive:
- Authentication headers and endpoint URLs for the external commerce API are centralized in one boundary adapter.
- Cache revalidation security is enforced consistently by isolating secret verification.
- Consuming modules remain decoupled from low-level network request construction.

Negative:
- All data fetching requirements must conform to the internal adapter boundary, introducing an additional abstraction layer.
- Changes to external GraphQL schema structures necessitate corresponding updates to internal client adapters.

## Alternatives

- Direct network fetch calls within individual components and route handlers (rejected)
  Rejected because: Scatters store domain configuration, access token exposure, and request payload formatting across multiple modules.
  When valid: Trivial prototype scripts requiring single-use standalone external access.
- External third-party vendor client SDK (rejected)
  Rejected because: Adds third-party package dependencies when lightweight HTTP POST calls with standardized headers fulfill integration requirements.
  When valid: Environments where an official vendor client provides required capabilities not easily implemented via standard HTTP calls.

## Risks

- Invalid or missing revalidation secrets may cause webhook handlers to fail or silently discard revalidation requests.
  Mitigation: Log authentication errors explicitly and enforce secret presence checks in runtime configuration.
  Owner: engineering team
- External API rate limiting or endpoint downtime impacts all internal client callers simultaneously.
  Mitigation: Implement centralized error logging and appropriate cache layers to mitigate external request volume.
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
- Retrieve environment credentials from runtime configuration sources rather than hardcoding endpoints or access keys.
- Structure external POST requests to encapsulate query string and variables serialization while standardizing error handling.

## Continuation Context


Verify commands:
- echo "Discover project test command from repository manifest and execute integration test suite"
- echo "Discover project static analysis script to verify external client boundary isolation"

Accept when:
- Outbound requests to the external commerce API include required authentication headers and execute through the internal adapter boundary.
- Webhook revalidation handlers reject requests with invalid secrets and log the condition.

## Enforcement

- Verified by: Automated test suites verifying external client request construction and webhook secret validation.
- Verified by: Code review checks ensuring no direct network calls bypass the internal client adapter boundary.
- Violation handling: Pull requests containing direct external calls or bypassed revalidation secrets are blocked from merging.
- Violation handling: Unvalidated external client calls must be refactored to use the central boundary adapter.
- Exception process: Exceptions require documented architectural approval and justification detailing why the standard client adapter cannot support the use case.