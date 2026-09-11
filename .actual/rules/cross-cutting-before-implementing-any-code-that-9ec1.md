# Runtime Configuration Sourcing via Process Environment Variables: Before Implementing Any Code That Relies

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Before implementing any code that relies on environment variables, the developer MUST discover the project's dependency manifest and lock file to determine the exact runtime environment version and consult its official documentation for environment variable handling.

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