# Access Runtime Configuration Through Process Environment Variables: Modules That Consume

These rules are ALWAYS ACTIVE for all modules that consume runtime configuration values for external service integration, deployment environment identification, and application branding.

### Rules

- **R-ENV-001** SHOULD: Modules that consume multiple related configuration values SHOULD encapsulate environment access in dedicated configuration retrieval functions.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable validation utilities or configuration modules that enumerate required keys.
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Locate and execute the project's test suite to verify that configuration access is properly mocked or isolated in test environments.
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | head -20

# Search the codebase for direct process environment access patterns and verify they align with documented configuration requirements.
grep -r "process\.env\|os\.environ\|ENV\[" --include="*.js" --include="*.ts" --include="*.py" --include="*.rb" | wc -l

# Verify configuration validation executes at application startup.
grep -r "validateConfig\|checkEnv\|validateEnvironment" --include="*.js" --include="*.ts" --include="*.py" | head -10
```

**Accept when:**
- All required environment variables are documented with clear descriptions of purpose, format, and impact when missing.
- Configuration validation executes at application startup and provides clear error messages identifying missing or invalid environment variables.
- Test suite executes successfully with proper environment isolation and does not leak configuration state between test cases.
- Direct process environment access is encapsulated in dedicated configuration retrieval functions rather than scattered throughout the codebase.
- No hardcoded credentials or environment-specific values are present in source code.

<enforcement>
Claude Code MUST NOT skip or defer verification. Configuration access patterns MUST be validated at startup with clear error messages. All required environment variables MUST be documented.
</enforcement>