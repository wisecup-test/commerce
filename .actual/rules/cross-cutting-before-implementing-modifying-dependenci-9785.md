---
version: 2
name: cross-cutting-before-implementing-modifying-dependenci-9785
description: "Shopify Storefront API Client Encapsulation: Before Implementing Modifying Dependencies External Integration"
metadata:
  actual:
    adr_id: 97853ea8-f465-4dc9-9f9c-64a8b24bc303
    status: accepted
    candidate_group_id: cdc033d6-461b-4495-949d-3f387bfbdc99
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/**
---

# Shopify Storefront API Client Encapsulation: Before Implementing Modifying Dependencies External Integration

Scope: applies to `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EXT-001** MUST: Before implementing or modifying dependencies for external integration, the consumer MUST inspect the repository lock artifact to determine the authoritative locked version and consult the official documentation for that resolved version.

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