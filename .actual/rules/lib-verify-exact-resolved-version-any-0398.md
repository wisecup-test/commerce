# Runtime Configuration Management via `process.env`: Verify Exact Resolved Version Any Versioned

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RCM-001** MUST: Verify the exact resolved version of any versioned dependency by inspecting the project's lock file before implementing code that uses it.

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