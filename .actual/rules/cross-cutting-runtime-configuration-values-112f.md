# Source Runtime Configuration from Process Environment Variables: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all server-side runtime code including API routes, server components, middleware, and external service integration layers that require access to environment-specific configuration and credentials.

### Rules

- **R-ENV-001** MUST: All runtime configuration values MUST be sourced from process environment variables accessed through the standard runtime environment interface.
- **R-ENV-002** MUST: Sensitive credentials including API tokens, authentication keys, and domain identifiers MUST NOT be hardcoded in source files or committed to version control.
- **R-ENV-003** MUST: Create validation utilities that check for required environment variables during application initialization and fail fast with clear error messages indicating which variables are missing.
- **R-ENV-004** SHOULD: Implement logging filters that redact known sensitive environment variable names to prevent accidental credential exposure in logs or error messages.
- **R-ENV-005** SHOULD: Document all required environment variables in deployment documentation, including their purpose, expected format, and whether they contain sensitive data.
- **R-ENV-006** MAY: Public, non-sensitive configuration values such as site names or public API endpoints may be embedded in client-side code if explicitly intended for public consumption and do not expose security boundaries.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Locate validation utilities or startup scripts that verify environment variable presence
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -i "validat\|check\|require" | head -10

# Search for all environment variable access patterns
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | wc -l

# Verify no sensitive credentials are hardcoded
grep -r "api[_-]?key\|secret\|password\|token" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -v "process\.env\|os\.environ\|getenv" | grep -v "node_modules" | grep -v ".git" | head -20

# Run tests with missing environment variables to verify error handling
env -i npm test 2>&1 | grep -i "missing\|required\|undefined" || echo "No validation errors detected"
```

**Accept when:**
- All environment variable access uses the standard runtime environment interface without hardcoded fallback values for sensitive credentials.
- Validation utilities or startup checks verify the presence of required environment variables and fail with clear error messages when configuration is incomplete.
- No sensitive credentials are hardcoded in source files or committed to version control.
- Configuration access patterns are consistent across all modules that require runtime configuration values.
- Logging and error handling code does not expose sensitive environment variable values in output.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that accesses runtime configuration. Violations involving hardcoded credentials or sensitive value exposure must be rejected immediately.
</enforcement>