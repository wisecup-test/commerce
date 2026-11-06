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

These rules are ALWAYS ACTIVE for outbound HTTP requests and GraphQL queries targeting the Shopify Storefront API, and inbound webhook verification and cache revalidation handlers associated with Shopify topics.

### Rules

- **R-SHOP-001** MUST: The internal client adapter MUST authenticate requests to the external endpoint by supplying the storefront access token via the X-Shopify-Storefront-Access-Token header and setting the Content-Type header to application/json.

### Verify

```bash
# Discover project test command from repository manifest and execute integration test suite
echo "Discover project test command from repository manifest and execute integration test suite"
# Discover project static analysis script to verify external client boundary isolation
echo "Discover project static analysis script to verify external client boundary isolation"
```

**Accept when:**
- Outbound requests to the external commerce API include required authentication headers and execute through the internal adapter boundary.
- Webhook revalidation handlers reject requests with invalid secrets and log the condition.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>