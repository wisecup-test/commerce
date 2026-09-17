# Runtime Configuration Management via `process.env`: Not Hardcode Sensitive Credentials Environment Specific

These rules are ALWAYS ACTIVE for application code that requires dynamic configuration values, especially sensitive credentials or environment-specific settings.

### Rules

- **R-CONF-001** MUST_NOT: hardcode sensitive credentials or environment-specific URLs directly in source code.

### Verify

```bash
# Discover the project's build scripts and execute them to ensure successful compilation.
# Example: npm run build

# Discover the project's test scripts and execute them to ensure all tests pass.
# Example: npm test

# Discover the project's deployment scripts and simulate a deployment to verify environment variable injection.
# This may involve inspecting deployed artifacts or runtime behavior in a test environment.
```

**Accept when:**
- Application builds successfully without errors related to configuration.
- All automated tests pass, including those that rely on configured values.
- The application starts and operates correctly in a deployed environment, accessing configured values as expected.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>