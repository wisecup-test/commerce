# Access Runtime Configuration Through Process Environment Variables: Source Code Not

These rules are ALWAYS ACTIVE for all source code files that access runtime configuration, external service integration parameters, deployment environment identification, application metadata, API credentials, and environment-specific URLs.

### Rules

- **R-ENV-001** MUST NOT: Source code MUST NOT contain hardcoded values for credentials, API keys, deployment URLs, or environment-specific endpoints that should be externalized to process environment configuration.
- **R-ENV-002** MUST: Create a centralized validation function that checks all required environment variables at application startup and throws descriptive errors for missing or invalid values, ensuring fail-fast behavior before request processing begins.
- **R-ENV-003** MUST: Document all required and optional environment variables in deployment documentation, including expected format, example values, and the impact of missing configuration on application behavior.
- **R-ENV-004** SHOULD: Implement a typed configuration object that wraps environment variable access and provides type conversion, validation, and default values in a single location rather than scattered access throughout the codebase.
- **R-ENV-005** SHOULD: Minimize the scope of credential access by encapsulating environment variable retrieval in dedicated modules and applying principle of least privilege in configuration access patterns.
- **R-ENV-006** SHOULD: Use test framework setup and teardown hooks to capture and restore environment state, or use dependency injection to provide test-specific configuration without mutating global state.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable validation utilities or configuration modules that enumerate required keys.
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Search the codebase for direct process environment access patterns
grep -r 'process\.env\|os\.environ\|getenv' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . 2>/dev/null | grep -v node_modules | grep -v '.git'

# Locate and execute the project's test suite to verify configuration access is properly mocked or isolated
find . -name 'jest.config.*' -o -name 'pytest.ini' -o -name 'vitest.config.*' -o -name '.mocharc.*' | head -3

# Search for hardcoded credentials or environment-specific values
grep -r 'api[_-]?key\|secret\|password\|token' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | grep -v node_modules | grep -v '.git' | grep -v 'process\.env' | grep -v 'os\.environ'

# Verify configuration validation at startup
grep -r 'startup\|initialize\|bootstrap' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | grep -i 'config\|env' | head -10
```

**Accept when:**
- All required environment variables are documented with clear descriptions of purpose, format, and impact when missing.
- Configuration validation executes at application startup and provides clear error messages identifying missing or invalid environment variables.
- Test suite executes successfully with proper environment isolation and does not leak configuration state between test cases.
- No hardcoded credentials, API keys, or environment-specific values are found in source code outside of test fixtures or documentation.
- All environment variable access is centralized in dedicated configuration modules or validation functions.

<enforcement>
Clause Code MUST NOT skip or defer verification of these rules. Static analysis and test execution are mandatory before accepting configuration access patterns.
</enforcement>