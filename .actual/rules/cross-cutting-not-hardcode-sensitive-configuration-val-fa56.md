# Environment Variable Management using `process.env`: Not Hardcode Sensitive Configuration Values Directly

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST_NOT: MUST NOT hardcode sensitive configuration values directly into source code.

### Verify

```bash
# Discover the project's build or runtime environment setup scripts.
# Execute the project's environment variable loading or validation scripts.
# Inspect the running application's configuration values.
```

**Accept when:**
- The application successfully starts and accesses all necessary configuration without errors.
- Sensitive values are correctly loaded from the environment and not exposed.
- Configuration values reflect the intended environment-specific settings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>