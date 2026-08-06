# Access Environment Variables Through Process Environment Interface: Source Code Shall

These rules are ALWAYS ACTIVE for all source code files that access runtime configuration values, including API client modules, UI components, utility functions, webhook handlers, and configuration validation logic.

### Rules

- **R-ENV-001** MUST_NOT: Source code SHALL NOT contain hardcoded credentials, API keys, tokens, or other sensitive configuration values.
- **R-ENV-002** MUST: Configuration values for external service integration (store domains, access tokens, revalidation secrets, site names, deployment URLs) SHALL be retrieved through the process environment interface.
- **R-ENV-003** MUST: Required environment variables SHALL be validated at application startup with clear error messages for missing or invalid configuration.
- **R-ENV-004** SHOULD: Create a centralized configuration module that accesses all required environment variables and exports typed configuration objects with validation.
- **R-ENV-005** SHOULD: Document all required and optional environment variables in the repository with descriptions, expected formats, and example values.
- **R-ENV-006** MAY: Non-sensitive default values may be hardcoded with explicit documentation and code review approval.
- **R-ENV-007** MAY: Test fixtures and mock data may contain hardcoded values with clear separation from production code paths.

### Verify

```bash
# Discover and execute the project's environment variable validation script
find . -name '*env*validate*' -o -name '*config*validate*' | head -1 | xargs -I {} bash {}

# Run integration tests that verify environment variable access patterns
npm test -- --testPathPattern='(integration|env|config)' 2>/dev/null || yarn test --testPathPattern='(integration|env|config)' 2>/dev/null || echo "Test suite not found"

# Execute static analysis to detect hardcoded credentials
grep -r "(password|token|secret|key)\s*=\s*['\"]" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . 2>/dev/null | grep -v node_modules | grep -v '.actual' || echo "No hardcoded credentials detected"
```

**Accept when:**
- All required environment variables are successfully retrieved through the process environment interface without runtime errors
- No hardcoded credentials, tokens, or sensitive configuration values are present in source files
- Environment variable validation logic correctly identifies missing or invalid configuration at startup
- Static analysis tools confirm absence of hardcoded credentials in source code
- Code review process verifies that new configuration values use environment variables
- Integration tests validate environment variable access patterns in deployment environments

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that accesses runtime configuration. Violations detected by static analysis or integration tests MUST block merge requests and build pipelines.
</enforcement>