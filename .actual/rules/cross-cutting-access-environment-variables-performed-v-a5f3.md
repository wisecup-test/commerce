# Runtime Configuration Sourcing via Process Environment Variables: Access Environment Variables Performed Via Process

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Access to environment variables MUST be performed via the `process.env` object.

### Verify

```bash
# Discover the project's build scripts and execute them to ensure successful compilation.
# Discover the project's test suite and run all tests to confirm application functionality.
# Inspect the deployed application's runtime environment to confirm environment variables are correctly set and accessed.
```

**Accept when:**
- The application successfully starts and operates without configuration-related errors.
- Sensitive information is not hardcoded in the codebase.
- Changes to environment variables correctly alter application behavior without code modification.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>