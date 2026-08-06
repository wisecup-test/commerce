# Source Runtime Configuration from Process Environment Variables: Configuration Access Use

These rules are ALWAYS ACTIVE for all server-side runtime code including API routes, server components, middleware, and external service integration layers that require access to environment-specific configuration and credentials.

### Rules

- **R-CONFIG-001** MUST: Configuration access MUST use direct property access on the environment object without intermediate caching layers at module initialization time.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate any validation utilities or startup scripts that verify environment variable presence
grep -r "process\.env\|ENV\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -i "validat\|check\|require" | head -20

# Search the codebase for all environment variable access patterns
grep -r "process\.env\|ENV\[\|getenv(" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | wc -l

# Verify that sensitive credentials are sourced exclusively from environment variables
grep -r "password\|token\|secret\|api[_-]?key\|credential" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -v "process\.env\|ENV\|getenv" | grep -v "node_modules" | grep -v ".git" | head -20

# Identify the project's test suite configuration
find . -name "*.test.js" -o -name "*.test.ts" -o -name "test_*.py" -o -name "*_test.go" | head -5
```

**Accept when:**
- All environment variable access uses the standard runtime environment interface without hardcoded fallback values for sensitive credentials.
- Validation utilities or startup checks verify the presence of required environment variables and fail with clear error messages when configuration is incomplete.
- No sensitive credentials are hardcoded in source files or committed to version control.
- Configuration access patterns are consistent across all modules that require runtime configuration values.
- Tests can be run with missing environment variables and produce appropriate error handling and validation.

<enforcement>
Claude Code MUST NOT skip or defer verification of environment variable access patterns and credential sourcing compliance.
</enforcement>