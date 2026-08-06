# Source Runtime Configuration from Process Environment Variables: Modules That Consume

These rules are ALWAYS ACTIVE for all server-side runtime code including API routes, server components, middleware, and modules that consume configuration values for external service integration, authentication tokens, endpoint URLs, and deployment-specific settings.

### Rules

- **R-ENV-001** SHOULD: Modules that consume configuration values SHOULD validate the presence and format of required environment variables at the point of use or during application initialization.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Locate any validation utilities or startup scripts that verify environment variable presence
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" | grep -i "validat\|check\|require" | head -20

# Search the codebase for all environment variable access patterns
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" | wc -l

# Verify that sensitive credentials are sourced exclusively from environment variables
grep -r "password\|token\|secret\|api[_-]?key\|credential" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" | grep -v "process\.env\|os\.environ\|getenv" | grep -v "test\|mock\|example" | head -10

# Identify the project's test suite configuration
find . -name 'jest.config.*' -o -name 'pytest.ini' -o -name 'vitest.config.*' -o -name '.mocharc.*' | head -5

# Run tests with missing environment variables to verify error handling
echo "Check test configuration for environment variable mocking and validation tests"
```

**Accept when:**
- All environment variable access uses the standard runtime environment interface without hardcoded fallback values for sensitive credentials.
- Validation utilities or startup checks verify the presence of required environment variables and fail with clear error messages when configuration is incomplete.
- No sensitive credentials are hardcoded in source files or committed to version control.
- Configuration access patterns are consistent across all modules that require runtime configuration values.
- Integration tests verify application behavior with missing or malformed environment variables.

<enforcement>
Claude Code MUST NOT skip or defer verification. All environment variable access patterns must be audited before accepting changes that introduce new configuration consumption.
</enforcement>