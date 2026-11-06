---
version: 2
name: cross-cutting-outbound-requests-shopify-storefront-rou-65bd
description: "Shopify Storefront API Client Encapsulation: Outbound Requests Shopify Storefront Route Through"
metadata:
  actual:
    adr_id: 65bd72e7-9bac-456c-8a3a-f5e7737e4f1b
    status: accepted
    candidate_group_id: cdc033d6-461b-4495-949d-3f387bfbdc99
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Storefront API Client Encapsulation: Outbound Requests Shopify Storefront Route Through

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOP-001** MUST: All outbound requests to the Shopify Storefront API MUST route through a dedicated internal client adapter that standardizes transport, authentication headers, and request formatting.

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