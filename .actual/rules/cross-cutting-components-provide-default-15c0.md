# Access Environment Variables Through Process Environment Interface: Components Provide Default

These rules are ALWAYS ACTIVE for API client modules, UI components, utility functions, webhook handlers, and configuration validation logic that require runtime configuration values for external service integration.

### Rules

- **R-ENV-001** MAY: Components MAY provide default fallback values for non-sensitive configuration when environment variables are undefined.
- **R-ENV-002** MUST: Create a centralized configuration module that accesses all required environment variables and exports typed configuration objects with validation.
- **R-ENV-003** MUST: Document all required and optional environment variables in the repository with descriptions, expected formats, and example values.
- **R-ENV-004** MUST: Use environment variable validation utilities to check for required values at application startup and provide clear error messages for missing or invalid configuration.
- **R-ENV-005** MUST: Implement logging filters and error handlers that redact sensitive environment variable values from all output.
- **R-ENV-006** MUST NOT: Hardcode sensitive credentials, tokens, or sensitive configuration values in source files.
- **R-ENV-007** MUST NOT: Embed environment-specific settings directly into source code without externalizing through the process environment interface.

### Verify

```bash
# Discover and execute the project's environment variable validation script
find . -name '*env*validate*' -o -name '*config*validate*' | head -1 | xargs bash

# Locate and run the project's test suite for integration tests
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' | head -1 | xargs -I {} dirname {} | xargs -I {} bash -c 'cd {} && npm test 2>/dev/null || python -m pytest 2>/dev/null || go test ./... 2>/dev/null'

# Execute static analysis to detect hardcoded credentials
find . -type f \( -name '*.js' -o -name '*.ts' -o -name '*.py' -o -name '*.go' \) -exec grep -l 'password\|token\|secret\|key.*=' {} \; | grep -v node_modules | grep -v '.git'
```

**Accept when:**
- All required environment variables are successfully retrieved through the process environment interface without runtime errors
- No hardcoded credentials, tokens, or sensitive configuration values are present in source files
- Environment variable validation logic correctly identifies missing or invalid configuration at startup
- Static analysis tools report zero hardcoded credentials in source files
- Integration tests pass for environment variable access patterns across all modules
- Documentation exists for all required and optional environment variables with expected formats

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code generation and modification within the configured scope.
</enforcement>