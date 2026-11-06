---
version: 2
name: cross-cutting-internal-adapter-encapsulate-session-coo-e94f
description: "Shopify Storefront API Client Encapsulation: Internal Adapter Encapsulate Session Cookie Retrieval"
metadata:
  actual:
    adr_id: e94ff188-fb02-4fc2-ae5d-68bdbee55418
    status: accepted
    candidate_group_id: cdc033d6-461b-4495-949d-3f387bfbdc99
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Storefront API Client Encapsulation: Internal Adapter Encapsulate Session Cookie Retrieval

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for outbound HTTP requests, GraphQL queries targeting the Shopify Storefront API, and inbound webhook verification and cache revalidation handlers associated with Shopify topics.

### Rules

- **R-SHOULD-001** SHOULD: The internal adapter encapsulate session cookie retrieval and request header parsing required for external API queries within boundary functions.

### Verify

```bash
echo "Discover project test command from repository manifest and execute integration test suite"
echo "Discover project static analysis script to verify external client boundary isolation"
```

**Accept when:**
- Outbound requests to the external commerce API include required authentication headers and execute through the internal adapter boundary.
- Webhook revalidation handlers reject requests with invalid secrets and log the condition.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>