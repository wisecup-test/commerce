---
version: 2
name: cross-cutting-route-handlers-not-construct-bespoke-93f2
description: "Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Route Handlers Not Construct Bespoke Decentralized"
metadata:
  actual:
    adr_id: 93f248eb-b383-4000-afc1-14861ca99fcf
    status: accepted
    candidate_group_id: f7137deb-eaa3-44c3-b6f2-14b4fdfa2721
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Route Handlers Not Construct Bespoke Decentralized

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for:
- All route-level social metadata and OpenGraph image generation handlers across root and dynamic route segments.

### Rules

- **R-LM-001** MUST_NOT: Route handlers MUST NOT construct bespoke, decentralized image presentation markup or inline graphic layouts that bypass the shared image module.

### Verify

- Discover the repository build and static analysis commands from package manifests and run lint checks across all route image handlers.
- Discover and execute the project test runner to verify that social image generation handlers resolve and execute without errors.

**Accept when:**

- Static analysis and lint checks complete with zero errors across all OpenGraph image route handlers.
- All OpenGraph image route endpoints successfully resolve and render preview images using the shared presentation module without interface or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and code review checks during continuous integration.
- Verified by: Component integration tests verifying social image route rendering contracts.
- Violation handling: Pull requests introducing decentralized image layout markup or bypassing the shared presentation module fail automated review gates.
- Violation handling: Violations must be refactored to consume the shared image component before branch merge.
- Exception process: Submit an architectural review request documenting why the shared image component cannot satisfy the visual or technical requirements of the route handler.
- Exception process: Obtain approval from frontend engineering leads before introducing bespoke image generation logic.
</enforcement>
