# Access Configuration Values Through Environment Variables: Configuration Variable Names

These rules are ALWAYS ACTIVE for all server-side request handling code, build-time functions, API client initialization, webhook validation, and any module requiring deployment-environment-specific values.

### Rules

- **R-CONFIG-001** SHOULD: Configuration variable names SHOULD use uppercase naming with underscore separators to distinguish them from code-level identifiers.
- **R-CONFIG-002** MUST: All configuration access MUST use the process environment interface pattern without hardcoded environment-specific values in source files.
- **R-CONFIG-003** MUST: Sensitive credentials including tokens and secrets MUST be accessed exclusively through environment variables.
- **R-CONFIG-004** SHOULD: Create a centralized configuration module that accesses all environment variables in one location, providing typed accessors and validation.
- **R-CONFIG-005** SHOULD: Implement a startup validation function that verifies all required environment variables are present and properly formatted before the application begins processing requests.
- **R-CONFIG-006** MUST: Document all required environment variables in deployment documentation, including their purpose, format requirements, and whether they contain sensitive data requiring secret management.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Search the codebase for all environment variable access patterns
grep -r 'process\.env\|os\.environ\|ENV\[' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' --include='*.rb' . 2>/dev/null | grep -v node_modules | head -20

# Verify that sensitive values are not hardcoded
grep -r 'SHOPIFY_STORE_DOMAIN\|SHOPIFY_STOREFRONT_ACCESS_TOKEN\|SHOPIFY_REVALIDATION_SECRET\|SITE_NAME' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . 2>/dev/null | grep -v 'process\.env\|os\.environ\|ENV\[' | grep -v node_modules

# Locate and inspect the configuration loading module
find . -name '*config*' -type f \( -name '*.js' -o -name '*.ts' -o -name '*.py' \) | grep -v node_modules | head -5

# Search for startup validation or configuration validation patterns
grep -r 'validate\|required\|missing' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | grep -i 'env\|config' | head -10
```

**Accept when:**
- All configuration access uses the process environment interface pattern without hardcoded environment-specific values in source files
- Sensitive credentials including tokens and secrets are accessed exclusively through environment variables
- The application provides clear error messages when required environment variables are missing or invalid
- A centralized configuration module exists with typed accessors and validation
- Startup validation fails fast with clear diagnostics when required environment variables are absent
- All required environment variables are documented in deployment guides

<enforcement>
Clause MUST NOT skip or defer verification. Code review MUST confirm no sensitive values are hardcoded. Static analysis MUST scan for hardcoded credentials. Integration tests MUST validate configuration loading with missing or malformed environment variables.
</enforcement>