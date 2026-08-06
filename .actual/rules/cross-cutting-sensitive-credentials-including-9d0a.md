# Access Environment Variables Through Process Environment Interface: Sensitive Credentials Including

These rules are ALWAYS ACTIVE for all files that access runtime configuration values, including API client modules, UI components, utility functions, webhook handlers, and configuration validation logic.

### Rules

- **R-CRED-001** MUST: Sensitive credentials including access tokens, API keys, and revalidation secrets SHALL be accessed exclusively through environment variables.
- **R-CRED-002** MUST: All required environment variables SHALL be validated at application startup with clear error messages for missing or invalid configuration.
- **R-CRED-003** MUST: No hardcoded credentials, tokens, or sensitive configuration values SHALL be present in source files.
- **R-CRED-004** SHOULD: Create a centralized configuration module that accesses all required environment variables and exports typed configuration objects with validation.
- **R-CRED-005** SHOULD: Document all required and optional environment variables in the repository with descriptions, expected formats, and example values.
- **R-CRED-006** MAY: Non-sensitive default values may be hardcoded with explicit documentation and code review approval.
- **R-CRED-007** MAY: Test fixtures and mock data may contain hardcoded values with clear separation from production code paths.

### Verify

```bash
# Discover and execute the project's environment variable validation script
find . -name '*env*validate*' -o -name '*config*validate*' | head -1 | xargs -I {} bash {}

# Run integration tests that verify environment variable access patterns
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' | head -1 | xargs -I {} sh -c 'cd $(dirname {}) && npm test 2>/dev/null || python -m pytest 2>/dev/null || go test ./... 2>/dev/null'

# Execute static analysis to detect hardcoded credentials
grep -r "(password|token|secret|api[_-]?key)\s*[=:]\s*['\"]" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" --include="*.py" --include="*.go" . 2>/dev/null || echo "No hardcoded credentials detected"
```

**Accept when:**
- All required environment variables are successfully retrieved through the process environment interface without runtime errors
- No hardcoded credentials, tokens, or sensitive configuration values are present in source files
- Environment variable validation logic correctly identifies missing or invalid configuration at startup
- Static analysis tools report zero hardcoded credentials in source files
- Integration tests pass with environment variables properly configured

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification commands MUST execute successfully before accepting changes that introduce or modify environment variable access patterns.
</enforcement>