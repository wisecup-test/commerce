# Source Runtime Configuration from Process Environment Variables: Utility Modules Provide

These rules are ALWAYS ACTIVE for all server-side runtime code including API routes, server components, middleware, and configuration access for external service integration.

### Rules

- **R-ENV-001** MAY: Utility modules MAY provide validation functions that verify the completeness and correctness of required environment variables before application startup.
- **R-ENV-002** MUST: All environment variable access for sensitive credentials (API tokens, authentication credentials, domain identifiers) MUST source exclusively from process environment variables without hardcoded fallback values.
- **R-ENV-003** MUST: No sensitive credentials or authentication tokens SHALL be hardcoded in source files or committed to version control.
- **R-ENV-004** SHOULD: Configuration access patterns SHOULD be consistent across all modules that require runtime configuration values.
- **R-ENV-005** SHOULD: Validation utilities or startup checks SHOULD verify the presence of required environment variables and fail with clear error messages when configuration is incomplete.
- **R-ENV-006** SHOULD: All required environment variables SHOULD be documented in deployment documentation, including their purpose, expected format, and whether they contain sensitive data.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Locate validation utilities or startup scripts that verify environment variable presence
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -i "validat\|check\|require" | head -20

# Search for all environment variable access patterns
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | wc -l

# Verify no hardcoded sensitive credentials in source files
grep -r "api[_-]?key\|secret\|password\|token" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -v "process\.env\|os\.environ\|getenv" | grep -v "test\|mock\|example" | head -20

# Check git history for accidentally committed credentials
git log -p --all -S "api_key\|secret_key\|password" -- . 2>/dev/null | head -50

# Identify test suite configuration
find . -name "*.test.js" -o -name "*.test.ts" -o -name "*_test.py" -o -name "*_test.go" | head -10
```

**Accept when:**
- All environment variable access uses the standard runtime environment interface (process.env, os.environ, getenv, etc.) without hardcoded fallback values for sensitive credentials.
- Validation utilities or startup checks verify the presence of required environment variables and fail with clear error messages when configuration is incomplete.
- No sensitive credentials are hardcoded in source files or committed to version control.
- Configuration access patterns are consistent across all modules that require runtime configuration values.
- All required environment variables are documented with their purpose, expected format, and sensitivity classification.
- Tests verify application behavior with missing or malformed environment variables.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that accesses runtime configuration. Violations involving hardcoded credentials or sensitive data exposure must be rejected immediately.
</enforcement>