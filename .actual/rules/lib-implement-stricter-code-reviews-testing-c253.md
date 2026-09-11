# Shopify Integration Logic Undergoes Significant Refinement: Implement Stricter Code Reviews Testing Changes

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: Implement stricter code reviews and testing for all changes made to the core Shopify integration logic.

### Verify

```bash
# (no verify commands defined)
```

**Accept when:**
- Stricter code review criteria are defined and consistently applied to changes in `lib/shopify/index.ts` and associated utility files.
- Automated test coverage for the Shopify integration logic is enhanced and passes for all changes.
- Changes to the core Shopify integration logic consistently receive mandatory senior developer review or a higher number of required approvals.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>