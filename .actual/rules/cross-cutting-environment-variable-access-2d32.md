# Access Environment Variables Through Process Environment Interface: Environment Variable Access

These rules are ALWAYS ACTIVE for API client modules, UI components, utility functions, webhook handlers, and configuration validation logic that require runtime configuration values for external service integration.

### Rules

- **R-ENV-001** MUST: Environment variable access SHALL use the standard property access pattern on the process environment object.
- **R-ENV-002** MUST: Create a centralized configuration module that accesses all required environment variables and exports typed configuration objects with validation.
- **R-ENV-003** MUST: Implement validation logic that checks required environment variables at application startup and fails fast with clear error messages.
- **R-ENV-004** MUST: Implement logging filters and error handlers that redact sensitive environment variable values from all output.
- **R-ENV-005** SHOULD: Create validation utilities that parse and verify environment variable formats, types, and constraints before use.
- **R-ENV-006** SHOULD: Document all required and optional environment variables in the repository with descriptions, expected formats, and example values.

### Verify

```bash
# Discover and execute the project's environment variable validation script
find . -name "*env*validate*" -o -name "*config*validate*" | head -1 | xargs bash

# Locate and run integration tests that verify environment variable access patterns
find . -name "*.test.*" -o -name "*.spec.*" | xargs grep -l "process.env\|environment" | head -5 | xargs npm test --

# Execute static analysis to detect hardcoded credentials
grep -r "['\"]\(sk_live\|pk_live\|token\|secret\|password\)['\"]" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . || echo "No hardcoded credentials detected"
```

**Accept when:**
- All required environment variables are successfully retrieved through the process environment interface without runtime errors
- No hardcoded credentials, tokens, or sensitive configuration values are present in source files
- Environment variable validation logic correctly identifies missing or invalid configuration at startup
- Sensitive values are redacted from logs and error messages
- A centralized configuration module exists that exports typed configuration objects

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan source files for hardcoded credentials. Code review MUST verify that new configuration values use environment variables. Integration tests MUST validate environment variable access patterns. Build pipeline MUST fail if static analysis detects hardcoded credentials. Runtime validation MUST fail fast at startup if required environment variables are missing.
</enforcement>