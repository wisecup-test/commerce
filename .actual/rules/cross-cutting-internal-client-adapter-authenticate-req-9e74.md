---
version: 2
name: cross-cutting-internal-client-adapter-authenticate-req-9e74
description: "Shopify Storefront API Client Encapsulation: Internal Client Adapter Authenticate Requests External"
metadata:
  actual:
    adr_id: 9e74a5a0-34c5-4d06-a9a9-676932e520d6
    status: accepted
    candidate_group_id: cdc033d6-461b-4495-949d-3f387bfbdc99
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Storefront API Client Encapsulation: Internal Client Adapter Authenticate Requests External

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for:
- Outbound HTTP requests and GraphQL queries targeting the Shopify Storefront API.
- Inbound webhook verification and cache revalidation handlers associated with Shopify topics.

### Rules

- **R-IA-001** MUST: The internal client adapter MUST authenticate requests to the external endpoint by supplying the storefront access token via the X-Shopify-Storefront-Access-Token header and setting the Content-Type header to application/json.

### Verify

- echo "Discover project test command from repository manifest and execute integration test suite"
- echo "Discover project static analysis script to verify external client boundary isolation"

**Accept when:**

- Outbound requests to the external commerce API include required authentication headers and execute through the internal adapter boundary.
- Webhook revalidation handlers reject requests with invalid secrets and log the condition.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated test suites verifying external client request construction and webhook secret validation.
- Verified by: Code review checks ensuring no direct network calls bypass the internal client adapter boundary.
- Violation handling: Pull requests containing direct external calls or bypassed revalidation secrets are blocked from merging.
- Violation handling: Unvalidated external client calls must be refactored to use the central boundary adapter.
- Exception process: Exceptions require documented architectural approval and justification detailing why the standard client adapter cannot support the use case.
</enforcement>
