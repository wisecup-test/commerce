# Access Runtime Configuration Through Process Environment Variables: Configuration Validation Occur

These rules are ALWAYS ACTIVE for all code that accesses runtime configuration values for external service integration, deployment environment identification, application metadata, API credentials, and environment-specific URLs and endpoints.

### Rules

- **R-CONFIG-001** SHOULD: Configuration validation SHOULD occur at application initialization to fail fast when required values are missing or malformed.
- **R-CONFIG-002** MUST: All required environment variables MUST be documented with clear descriptions of purpose, format, and impact when missing.
- **R-CONFIG-003** SHOULD: Consider implementing a typed configuration object that wraps environment variable access and provides type conversion, validation, and default values in a single location.
- **R-CONFIG-004** MUST: Configuration access patterns MUST be encapsulated in dedicated modules to minimize scope of credential access and apply principle of least privilege.
- **R-CONFIG-005** MUST: Test suite setup and teardown hooks MUST capture and restore environment state to prevent test interdependencies and non-deterministic failures.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable validation utilities
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Locate configuration modules that enumerate required keys
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" | grep -E "(validation|config|initialize)" | head -10

# Execute the project's test suite to verify configuration isolation
npm test 2>&1 | grep -E "(PASS|FAIL|Error)" || echo "Test command not found"

# Search for direct process environment access patterns
grep -r "process\.env\[" --include="*.js" --include="*.ts" | wc -l

# Verify no hardcoded credentials in source
grep -r "password\|secret\|token\|key" --include="*.js" --include="*.ts" --include="*.py" | grep -v "process\.env" | grep -v "//" | head -5
```

**Accept when:**
- All required environment variables are documented with clear descriptions of purpose, format, and impact when missing.
- Configuration validation executes at application startup and provides clear error messages identifying missing or invalid environment variables.
- Test suite executes successfully with proper environment isolation and does not leak configuration state between test cases.
- Configuration access is encapsulated in dedicated modules rather than scattered throughout the codebase.
- No hardcoded credentials or environment-specific values are present in source code.

<enforcement>
Claude Code MUST NOT skip or defer verification. Configuration validation at startup is mandatory; missing or misconfigured environment variables MUST be caught before request processing begins.
</enforcement>