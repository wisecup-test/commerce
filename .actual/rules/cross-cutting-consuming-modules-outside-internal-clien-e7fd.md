---
version: 2
name: cross-cutting-consuming-modules-outside-internal-clien-e7fd
description: "Shopify Storefront API Client Encapsulation: Consuming Modules Outside Internal Client Boundary"
metadata:
  actual:
    adr_id: e7fd2ed3-0c99-4ab3-b4ad-0e487fad3a99
    status: accepted
    candidate_group_id: cdc033d6-461b-4495-949d-3f387bfbdc99
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Storefront API Client Encapsulation: Consuming Modules Outside Internal Client Boundary

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for outbound HTTP requests, GraphQL queries targeting the Shopify Storefront API, and inbound webhook verification and cache revalidation handlers associated with Shopify topics.

### Rules

- **R-SHOPIFY-001** MUST_NOT: Consuming modules outside the internal client boundary MUST NOT construct direct network requests or inject authentication tokens for the Shopify Storefront API.

### Verify

```bash
echo "Discover project test command from repository manifest and execute integration test suite"
echo "Discover project static analysis script to verify external client boundary isolation"
```

**Accept when:**
- Outbound requests to the external commerce API include required authentication headers and execute through the internal adapter boundary.
- Webhook revalidation handlers reject requests with invalid secrets and log the condition.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites and code review checks verify external client request construction and webhook secret validation.
</enforcement>