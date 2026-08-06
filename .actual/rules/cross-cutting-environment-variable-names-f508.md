# Access Environment Variables Through Process Environment Interface: Environment Variable Names

These rules are ALWAYS ACTIVE for all files matching the configured scope: API client modules, UI components, utility functions, webhook handlers, and configuration validation logic that access runtime configuration values.

### Rules

- **R-ENV-001** SHOULD: Environment variable names SHOULD follow uppercase snake_case naming convention with domain-specific prefixes.
- **R-ENV-002** MUST: Create a centralized configuration module that accesses all required environment variables and exports typed configuration objects with validation.
- **R-ENV-003** MUST: Document all required and optional environment variables in the repository with descriptions, expected formats, and example values.
- **R-ENV-004** MUST: Use environment variable validation utilities to check for required values at application startup and provide clear error messages for missing or invalid configuration.
- **R-ENV-005** MUST: Implement validation logic that checks required environment variables at application startup and fails fast with clear error messages.
- **R-ENV-006** MUST: Create validation utilities that parse and verify environment variable formats, types, and constraints before use.
- **R-ENV-007** MUST: Implement logging filters and error handlers that redact sensitive environment variable values from all output.

### Verify

```bash
# Discover the project's environment variable validation script in the repository and execute it
# to verify all required configuration values are defined
find . -type f -name '*env*' -o -name '*config*' | grep -E '(validate|check|verify)' | head -5

# Locate the project's test suite and run integration tests that verify environment variable
# access patterns across all modules
find . -type f \( -name '*.test.*' -o -name '*.spec.*' -o -name '*test*' \) | grep -i env | head -5

# Identify the project's static analysis or linting configuration and execute checks that
# detect hardcoded credentials or configuration values in source files
grep -r 'process\.env\|process\.ENV' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' | wc -l

# Verify no hardcoded credentials in source files
grep -r 'password\|token\|secret\|key' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' | grep -v 'process\.env' | grep -v 'node_modules' | wc -l
```

**Accept when:**
- All required environment variables are successfully retrieved through the process environment interface without runtime errors
- No hardcoded credentials, tokens, or sensitive configuration values are present in source files
- Environment variable validation logic correctly identifies missing or invalid configuration at startup
- A centralized configuration module exists that exports typed configuration objects
- All environment variables follow uppercase snake_case naming convention with domain-specific prefixes
- Documentation exists for all required and optional environment variables with descriptions and example values

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan source files for hardcoded credentials. Code review MUST verify new configuration values use environment variables. Integration tests MUST validate environment variable access patterns. Build pipeline MUST fail if hardcoded credentials are detected. Runtime validation MUST fail fast at startup if required environment variables are missing.
</enforcement>