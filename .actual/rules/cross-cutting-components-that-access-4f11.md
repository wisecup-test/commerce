# Access Runtime Configuration Through Process Environment Variables: Components That Access

These rules are ALWAYS ACTIVE for all components that access runtime configuration through process environment variables, including service clients, metadata generation components, utility functions, and application initialization code.

### Rules

- **R-ENV-001** MUST: Components that access process environment configuration MUST handle missing or invalid values through validation or default value assignment.
- **R-ENV-002** MUST: Create a centralized validation function that checks all required environment variables at application startup and throws descriptive errors for missing or invalid values, ensuring fail-fast behavior before request processing begins.
- **R-ENV-003** MUST: Document all required and optional environment variables in deployment documentation, including expected format, example values, and the impact of missing configuration on application behavior.
- **R-ENV-004** SHOULD: Implement a typed configuration object that wraps environment variable access and provides type conversion, validation, and default values in a single location rather than scattered access throughout the codebase.
- **R-ENV-005** MUST: Use test framework setup and teardown hooks to capture and restore environment state, or use dependency injection to provide test-specific configuration without mutating global state.
- **R-ENV-006** MUST: Encapsulate environment variable retrieval in dedicated modules and apply principle of least privilege in configuration access patterns to minimize credential exposure.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable validation utilities or configuration modules that enumerate required keys.
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Locate and execute the project's test suite to verify that configuration access is properly mocked or isolated in test environments.
grep -r "process.env\|os.environ\|ENV\[" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -E "(test|spec)" | head -10

# Search the codebase for direct process environment access patterns and verify they align with documented configuration requirements.
grep -r "process.env\|os.environ\|ENV\[" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -v node_modules | grep -v ".git"

# Verify configuration validation executes at application startup.
grep -r "startup\|initialize\|bootstrap" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -i "config\|env" | head -10

# Check for hardcoded credentials or environment-specific values.
grep -r "password\|secret\|token\|api.key" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . | grep -v "process.env\|os.environ\|ENV\[" | grep -v node_modules | head -10
```

**Accept when:**
- All required environment variables are documented with clear descriptions of purpose, format, and impact when missing.
- Configuration validation executes at application startup and provides clear error messages identifying missing or invalid environment variables.
- Test suite executes successfully with proper environment isolation and does not leak configuration state between test cases.
- All environment variable access is either through a centralized configuration module or explicitly validated at the point of access.
- No hardcoded credentials or environment-specific values are present in source code outside of test fixtures or documentation.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules R-ENV-001 through R-ENV-006 are mandatory and must be verified before accepting code that accesses process environment configuration.
</enforcement>