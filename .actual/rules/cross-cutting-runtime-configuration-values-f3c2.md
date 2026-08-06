# Access Runtime Configuration Through Process Environment Variables: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all code that accesses runtime configuration values for external service integration, deployment environment identification, application branding, API credentials, and environment-specific URLs.

### Rules

- **R-CONFIG-001** MUST: All runtime configuration values that vary across deployment environments or contain credentials SHALL be accessed through the process environment namespace.
- **R-CONFIG-002** MUST: Create a centralized validation function that checks all required environment variables at application startup and throws descriptive errors for missing or invalid values, ensuring fail-fast behavior before request processing begins.
- **R-CONFIG-003** MUST: Document all required and optional environment variables in deployment documentation, including expected format, example values, and the impact of missing configuration on application behavior.
- **R-CONFIG-004** SHOULD: Implement a typed configuration object that wraps environment variable access and provides type conversion, validation, and default values in a single location rather than scattered access throughout the codebase.
- **R-CONFIG-005** MUST: Encapsulate environment variable retrieval in dedicated modules to minimize the scope of credential access and apply principle of least privilege in configuration access patterns.
- **R-CONFIG-006** MUST: Use test framework setup and teardown hooks to capture and restore environment state, or use dependency injection to provide test-specific configuration without mutating global state.

### Verify

```bash
# Discover the project's dependency manifest and identify environment variable validation utilities
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Locate configuration modules that enumerate required keys
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -E "(config|environment|settings)" | head -10

# Search for centralized validation functions
grep -r "validate.*env\|check.*env\|required.*env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | head -10

# Verify test suite executes with proper environment isolation
grep -r "beforeEach\|afterEach\|setup\|teardown" --include="*.test.js" --include="*.test.ts" --include="*.spec.js" --include="*.spec.ts" | grep -i "env" | head -10

# Search for hardcoded credentials or environment-specific values
grep -r "http://localhost\|https://api\.\|api_key.*=\|secret.*=" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -v "process\.env" | head -10
```

**Accept when:**
- All required environment variables are documented with clear descriptions of purpose, format, and impact when missing.
- Configuration validation executes at application startup and provides clear error messages identifying missing or invalid environment variables.
- Test suite executes successfully with proper environment isolation and does not leak configuration state between test cases.
- All runtime configuration access is routed through process environment variables rather than hardcoded values.
- Centralized configuration validation module exists and is invoked at application startup.
- No hardcoded credentials or environment-specific values are present in source code.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review approval and deployment pipeline validation.
</enforcement>