# Access Runtime Configuration Through Process Environment Variables: Configuration Keys Accessed

These rules are ALWAYS ACTIVE for all code that accesses runtime configuration values from the process environment namespace, including service clients, metadata generation components, utility functions, and application initialization logic.

### Rules

- **R-CONFIG-001** MUST: Configuration keys accessed from the process environment MUST use consistent naming conventions that clearly identify the configuration domain and purpose.
- **R-CONFIG-002** MUST: Create a centralized validation function that checks all required environment variables at application startup and throws descriptive errors for missing or invalid values, ensuring fail-fast behavior before request processing begins.
- **R-CONFIG-003** MUST: Document all required and optional environment variables in deployment documentation, including expected format, example values, and the impact of missing configuration on application behavior.
- **R-CONFIG-004** SHOULD: Implement a typed configuration object that wraps environment variable access and provides type conversion, validation, and default values in a single location rather than scattered access throughout the codebase.
- **R-CONFIG-005** SHOULD: Minimize the scope of credential access by encapsulating environment variable retrieval in dedicated modules and applying principle of least privilege in configuration access patterns.
- **R-CONFIG-006** SHOULD: Use test framework setup and teardown hooks to capture and restore environment state, or use dependency injection to provide test-specific configuration without mutating global state.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable validation utilities or configuration modules that enumerate required keys.
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Search the codebase for direct process environment access patterns
grep -r 'process\.env\|os\.environ\|ENV\[' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' --include='*.rb' | head -20

# Locate and execute the project's test suite to verify configuration access is properly mocked or isolated
find . -name 'jest.config.*' -o -name 'pytest.ini' -o -name '.mocharc.*' -o -name 'vitest.config.*' | head -5

# Search for centralized configuration validation or initialization modules
find . -type f \( -name '*config*' -o -name '*env*' \) -path '*/src/*' -o -path '*/lib/*' | grep -E '\.(js|ts|py|go|rb)$' | head -10

# Verify no hardcoded credentials or environment-specific values in source code
grep -r 'password\|secret\|token\|api[_-]?key' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' --include='*.rb' | grep -v 'process\.env\|os\.environ\|ENV\[' | head -10
```

**Accept when:**
- All required environment variables are documented with clear descriptions of purpose, format, and impact when missing.
- Configuration validation executes at application startup and provides clear error messages identifying missing or invalid environment variables.
- Test suite executes successfully with proper environment isolation and does not leak configuration state between test cases.
- All configuration access patterns use consistent naming conventions that clearly identify the configuration domain and purpose.
- Centralized configuration validation function exists and is invoked before request processing begins.
- No hardcoded credentials or environment-specific values are present in source code.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that accesses process environment variables for runtime configuration.
</enforcement>