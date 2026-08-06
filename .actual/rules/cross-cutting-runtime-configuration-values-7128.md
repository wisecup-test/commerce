# Access Environment Variables Through Process Environment Interface: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all files matching the configured scope: API client modules, UI components, utility functions, webhook handlers, and configuration validation logic that require runtime configuration values for external service integration.

### Rules

- **R-ENV-001** MUST: All runtime configuration values SHALL be retrieved through the process environment interface rather than hardcoded in source files.
- **R-ENV-002** MUST: Create a centralized configuration module that accesses all required environment variables and exports typed configuration objects with validation.
- **R-ENV-003** MUST: Implement validation logic that checks required environment variables at application startup and fails fast with clear error messages.
- **R-ENV-004** MUST: Implement logging filters and error handlers that redact sensitive environment variable values from all output.
- **R-ENV-005** SHOULD: Create validation utilities that parse and verify environment variable formats, types, and constraints before use.
- **R-ENV-006** SHOULD: Document all required and optional environment variables in the repository with descriptions, expected formats, and example values.

### Verify

```bash
# Discover and execute the project's environment variable validation script
find . -name "*env*validate*" -o -name "*config*validate*" | head -1 | xargs bash

# Locate and run the project's test suite for integration tests
find . -name "package.json" -o -name "pyproject.toml" -o -name "go.mod" | head -1 | xargs -I {} dirname {} | xargs -I {} bash -c "cd {} && npm test 2>/dev/null || python -m pytest 2>/dev/null || go test ./... 2>/dev/null"

# Execute static analysis to detect hardcoded credentials
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | wc -l
grep -r "password\|token\|secret\|key" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -v "process\.env" | grep -v "os\.environ" | grep -v "getenv" | wc -l
```

**Accept when:**
- All required environment variables are successfully retrieved through the process environment interface without runtime errors
- No hardcoded credentials, tokens, or sensitive configuration values are present in source files
- Environment variable validation logic correctly identifies missing or invalid configuration at startup
- Static analysis detects zero hardcoded credentials in production code paths
- Integration tests pass with environment variables properly configured
- Logging output contains no exposed sensitive environment variable values

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that accesses runtime configuration values. Violations must be caught during code review and static analysis before merge.
</enforcement>