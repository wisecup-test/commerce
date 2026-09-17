# Runtime Configuration Management via `process.env`: Use Process Env Access Runtime Configuration

These rules are ALWAYS ACTIVE for all application modules and code requiring runtime configuration.

### Rules

- **R-PROCENV-001** MUST: Use `process.env` to access runtime configuration values.

### Verify

```bash
# Discover the project's build scripts and execute them to ensure successful compilation.
# Discover the project's test scripts and execute them to ensure all tests pass.
# Discover the project's deployment scripts and simulate a deployment to verify environment variable injection.
```

**Accept when:**
- Application builds successfully without errors related to configuration.
- All automated tests pass, including those that rely on configured values.
- The application starts and operates correctly in a deployed environment, accessing configured values as expected.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>