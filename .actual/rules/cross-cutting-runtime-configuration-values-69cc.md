# Use Process Environment Variables as Primary Runtime Configuration Source: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all server-side runtime contexts including API routes, server components, middleware, image generation functions, and any code that requires runtime configuration values for external service integration, authentication credentials, or environment-specific settings.

### Rules

- **R-CONFIG-001** MUST: All runtime configuration values SHALL be read from process environment variables using the standard process environment accessor.
- **R-CONFIG-002** MUST: No credentials, authentication tokens, or environment-specific values SHALL be hardcoded in source files.
- **R-CONFIG-003** MUST: Configuration variable names SHALL use consistent uppercase-with-underscores naming convention with descriptive prefixes.
- **R-CONFIG-004** MUST: Startup validation SHALL check for required environment variables before the application begins serving requests, failing fast with actionable error messages.
- **R-CONFIG-005** SHOULD: Create a centralized configuration module that accesses all environment variables through typed accessor functions with validation.
- **R-CONFIG-006** SHOULD: Document all required and optional environment variables in a dedicated configuration reference, including expected formats, example values, and validation rules.
- **R-CONFIG-007** SHOULD: Implement logging filters to redact sensitive values and avoid including configuration in error messages.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1

# Locate startup validation or configuration loading scripts
find . -type f -name '*config*' -o -name '*startup*' -o -name '*init*' | grep -E '\.(js|ts|py|go|rb)$' | head -10

# Search for direct process environment accessor usage
grep -r 'process\.env\|os\.environ\|getenv' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . 2>/dev/null | grep -v node_modules | head -20

# Verify no hardcoded credentials or environment-specific values
grep -r 'password\|secret\|token\|api[_-]?key' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . 2>/dev/null | grep -v node_modules | grep -v '\.env' | grep -v 'process\.env' | grep -v 'os\.environ' | head -20

# Identify the project's testing framework and locate configuration tests
grep -E '"test"|"jest"|"mocha"|"pytest"|"unittest"' package.json 2>/dev/null || echo 'Testing framework not found in package.json'

find . -type f -name '*.test.*' -o -name '*.spec.*' | grep -i config | head -10
```

**Accept when:**
- All runtime configuration values are sourced from process environment variables with no hardcoded credentials or environment-specific values in source code
- Configuration variable names use consistent uppercase-with-underscores naming convention and include descriptive prefixes
- Startup validation exists that checks for required environment variables and provides clear error messages for missing or invalid configuration
- A centralized configuration module or accessor pattern is implemented for typed access to environment variables
- Configuration reference documentation exists listing all required and optional environment variables with formats and examples
- No direct hardcoded secrets, tokens, API keys, or environment-specific URLs appear in source files outside of configuration accessors

<enforcement>
Claude Code MUST NOT skip or defer verification. All runtime configuration access MUST be validated against these rules before code is committed. Static analysis for hardcoded secrets and configuration values is mandatory.
</enforcement>