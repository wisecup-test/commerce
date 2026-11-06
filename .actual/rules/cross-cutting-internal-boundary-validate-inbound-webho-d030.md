---
version: 2
name: cross-cutting-internal-boundary-validate-inbound-webho-d030
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

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: The internal boundary MUST validate inbound webhook revalidation secrets prior to triggering cache tag invalidation or data revalidation routines.

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