# Runtime Configuration Sourcing via Process Environment Variables: Environment Variable Names Descriptive Follow Consistent

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: Environment variable names SHOULD be descriptive and follow a consistent naming convention (e.g., `UPPER_SNAKE_CASE`).

### Verify

```bash
Discover the project's build scripts and execute them to ensure successful compilation.
Discover the project's test suite and run all tests to confirm application functionality.
Inspect the deployed application's runtime environment to confirm environment variables are correctly set and accessed.
```

**Accept when:**
- The application successfully starts and operates without configuration-related errors.
- Sensitive information is not hardcoded in the codebase.
- Changes to environment variables correctly alter application behavior without code modification.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>