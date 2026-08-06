# Access Configuration Values Through Environment Variables: Modules Requiring Configuration

These rules are ALWAYS ACTIVE for all server-side request handling code, build-time functions, API client initialization, webhook validation, and any module requiring deployment-environment-specific values.

### Rules

- **R-CONFIG-001** SHOULD: Modules requiring configuration values SHOULD access them at the point of use rather than caching them at module initialization.
- **R-CONFIG-002** MUST: All configuration access MUST use the process environment interface pattern without hardcoded environment-specific values in source files.
- **R-CONFIG-003** MUST: Sensitive credentials including tokens and secrets MUST be accessed exclusively through environment variables.
- **R-CONFIG-004** SHOULD: Create a centralized configuration module that accesses all environment variables in one location, providing typed accessors and validation.
- **R-CONFIG-005** SHOULD: Implement a startup validation function that verifies all required environment variables are present and properly formatted before the application begins processing requests.
- **R-CONFIG-006** MUST: Document all required environment variables in deployment documentation, including their purpose, format requirements, and whether they contain sensitive data requiring secret management.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate any startup validation or configuration loading module and verify required environment variables are documented
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -E "(SHOPIFY_STORE_DOMAIN|SHOPIFY_STOREFRONT_ACCESS_TOKEN|SHOPIFY_REVALIDATION_SECRET|SITE_NAME)" | head -20

# Search the codebase for all environment variable access patterns
grep -r "process\.env\|getenv\|ENV\[" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" --include="*.py" --include="*.go" | wc -l

# Verify that sensitive values are not hardcoded in source files
grep -r "SHOPIFY_STORE_DOMAIN\s*=\|SHOPIFY_STOREFRONT_ACCESS_TOKEN\s*=\|SHOPIFY_REVALIDATION_SECRET\s*=" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" --include="*.py" --include="*.go" | grep -v "process\.env" | grep -v "getenv" | grep -v "ENV\[" || echo "No hardcoded credentials found"

# Locate the project's testing configuration and verify integration tests for configuration loading
find . -name '*.test.*' -o -name '*.spec.*' -o -name 'test' -type d | head -5
```

**Accept when:**
- All configuration access uses the process environment interface pattern without hardcoded environment-specific values in source files
- Sensitive credentials including tokens and secrets are accessed exclusively through environment variables
- The application provides clear error messages when required environment variables are missing or invalid
- A centralized configuration module exists with typed accessors and validation
- Startup validation fails fast with clear diagnostics when required environment variables are missing
- All required environment variables are documented in deployment guides

<enforcement>
Clause MUST NOT skip or defer verification. Code review MUST confirm no sensitive values are hardcoded. Static analysis MUST scan for hardcoded credentials. Integration tests MUST validate application startup behavior with missing environment variables.
</enforcement>