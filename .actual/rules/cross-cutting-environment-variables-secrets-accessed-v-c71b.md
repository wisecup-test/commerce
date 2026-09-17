# Adoption of Core Application Libraries and Next.js Modules: Environment Variables Secrets Accessed Via Standard

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Environment variables and secrets MUST be accessed via the standard process environment interface.

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