# Source Runtime Configuration from Process Environment Variables: Configuration Values Not

These rules are ALWAYS ACTIVE for all server-side runtime code including API routes, server components, middleware, and configuration access for external service integration.

### Rules

- **R-CONFIG-001** MUST NOT: Configuration values MUST NOT be logged, serialized to client-side code, or exposed through public API responses unless explicitly intended for public consumption.
- **R-CONFIG-002** MUST: Access environment variables directly at the point of use rather than caching them at module initialization to ensure configuration changes in test environments are properly reflected.
- **R-CONFIG-003** MUST: Create a validation utility that checks for required environment variables and provides clear error messages indicating which variables are missing and where they should be configured.
- **R-CONFIG-004** MUST: Document all required environment variables in deployment documentation, including their purpose, expected format, and whether they contain sensitive data that requires secure storage.
- **R-CONFIG-005** SHOULD: Consider implementing a type-safe configuration layer that wraps environment variable access with parsing and validation logic while maintaining the underlying environment variable source.
- **R-CONFIG-006** MUST: Implement logging filters that redact known sensitive environment variable names to prevent accidental exposure through error messages or debug output.
- **R-CONFIG-007** MUST: Ensure no sensitive credentials are hardcoded in source files or committed to version control.
- **R-CONFIG-EXC-001** MAY: Public, non-sensitive configuration values such as site names or public API endpoints may be embedded in client-side code if they are explicitly intended for public consumption and do not expose security boundaries.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Locate any validation utilities or startup scripts that verify environment variable presence
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -i "validat\|check\|require" | head -20

# Search the codebase for all environment variable access patterns
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | wc -l

# Verify that sensitive credentials are not hardcoded
grep -r "password\|token\|secret\|api[_-]?key" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -v "process\.env\|os\.environ\|getenv" | grep -v "test\|spec\|mock" | head -20

# Check for credentials in version control history
git log -p --all -S "password\|token\|secret\|api[_-]?key" | head -50

# Identify the project's test suite configuration
find . -name "*.test.js" -o -name "*.spec.js" -o -name "test_*.py" -o -name "*_test.go" | head -10

# Run tests with missing environment variables to verify error handling
echo "Environment variable validation test: set required vars to empty and run test suite"
```

**Accept when:**
- All environment variable access uses the standard runtime environment interface without hardcoded fallback values for sensitive credentials.
- Validation utilities or startup checks verify the presence of required environment variables and fail with clear error messages when configuration is incomplete.
- No sensitive credentials are hardcoded in source files or committed to version control.
- Configuration access patterns are consistent across all modules that require runtime configuration values.
- Logging filters are in place to redact sensitive environment variable names from output.
- Integration tests verify application behavior with missing or malformed environment variables.

<enforcement>
Clause Code MUST NOT skip or defer verification of environment variable sourcing patterns, credential exposure checks, and validation utility presence. All rules in this file are mandatory for code touching runtime configuration access.
</enforcement>