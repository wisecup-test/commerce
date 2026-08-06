# Access Configuration Values Through Environment Variables: Deployment Specific Configuration

These rules are ALWAYS ACTIVE for all server-side request handling code, build-time functions, API client initialization, webhook validation, and any module requiring deployment-environment-specific values.

### Rules

- **R-CONFIG-001** MUST: All deployment-specific configuration values SHALL be accessed through the process environment interface rather than hardcoded in source files.
- **R-CONFIG-002** MUST: Sensitive credentials including tokens, secrets, and API keys SHALL be accessed exclusively through environment variables and never embedded in source code.
- **R-CONFIG-003** MUST: Create a centralized configuration module that accesses all environment variables in one location, providing typed accessors and validation.
- **R-CONFIG-004** MUST: Implement startup validation that verifies all required environment variables are present and properly formatted before the application begins processing requests.
- **R-CONFIG-005** MUST: Provide clear error messages when required environment variables are missing or invalid, enabling fail-fast behavior with clear diagnostics.
- **R-CONFIG-006** SHOULD: Document all required environment variables in deployment documentation, including their purpose, format requirements, and whether they contain sensitive data requiring secret management.
- **R-CONFIG-007** SHOULD: Implement logging filters that redact environment variable values to prevent credential exposure through logs or error messages.
- **R-CONFIG-008** SHOULD: Add explicit type validation and parsing for configuration values at access points to prevent subtle bugs from type coercion or undefined handling.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1

# Locate any startup validation or configuration loading module
find . -type f \( -name '*config*' -o -name '*env*' \) -path '*/src/*' | grep -E '\.(ts|js|py|go)$'

# Search the codebase for all environment variable access patterns
grep -r 'process\.env\|os\.environ\|getenv' --include='*.ts' --include='*.js' --include='*.py' --include='*.go' . | grep -v node_modules | grep -v '.next'

# Verify that sensitive values are not hardcoded
grep -r 'SHOPIFY_STORE_DOMAIN\|SHOPIFY_STOREFRONT_ACCESS_TOKEN\|SHOPIFY_REVALIDATION_SECRET\|SITE_NAME' --include='*.ts' --include='*.js' . | grep -v 'process\.env' | grep -v node_modules | grep -v '.next'

# Locate the project's testing configuration and integration tests
find . -type f \( -name '*.test.*' -o -name '*.spec.*' -o -name 'jest.config.*' -o -name 'vitest.config.*' \) | head -5

# Search for configuration validation tests
grep -r 'environment\|config\|startup' --include='*.test.*' --include='*.spec.*' . | grep -i 'missing\|invalid\|required' | head -10
```

**Accept when:**
- All configuration access uses the process environment interface pattern without hardcoded environment-specific values in source files
- Sensitive credentials including tokens and secrets are accessed exclusively through environment variables
- A centralized configuration module exists that provides typed accessors and validation
- The application provides clear error messages when required environment variables are missing or invalid
- Startup validation is implemented and fails fast with clear diagnostics
- All required environment variables are documented in deployment guides
- No hardcoded sensitive values are found in source code through static analysis
- Integration tests validate configuration loading behavior with missing or malformed environment variables

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that accesses deployment-specific configuration. Static analysis and integration tests MUST pass before code is accepted.
</enforcement>