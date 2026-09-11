# Environment Variable Management using `process.env`: Verify Exact Resolved Version Any Versioned

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- R-ENV-VER-001 MUST: Verify the exact resolved version of any versioned library or framework used in conjunction with environment variables by inspecting the project's lock file before implementation.

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