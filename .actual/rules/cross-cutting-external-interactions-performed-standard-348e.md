# Adoption of Core Application Libraries and Next.js Modules: External Interactions Performed Standard Fetch Mechanism

These rules are ALWAYS ACTIVE for all code interacting with external APIs, requiring foundational utilities, or using Next.js modules for caching/context.

### Rules

- **R-FETCH-001** MUST: All external API interactions MUST be performed using the standard fetch mechanism.

### Verify

```bash
# Inspect the project's dependency manifest and lock file to confirm the presence and versions of the core libraries and framework modules.
# Review code changes to ensure new implementations adhere to the specified usage patterns for caching, headers, and external calls.
# Run the project's test suite to validate functionality that relies on these foundational components.
```

**Accept when:**
- The dependency manifest and lock file reflect the expected core libraries and framework modules.
- Code reviews confirm adherence to the established usage patterns for data access, caching, and external communication.
- All relevant tests pass, demonstrating correct integration and functionality.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>