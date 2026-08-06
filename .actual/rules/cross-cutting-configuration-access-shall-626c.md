# Access Configuration Values Through Environment Variables: Configuration Access Shall

These rules are ALWAYS ACTIVE for all server-side request handling code, build-time functions, API client initialization, webhook validation, and any module requiring deployment-environment-specific values.

### Rules

- **R-CONFIG-001** MUST: Configuration access SHALL use the standard process environment property accessor pattern for all server-side request handling code requiring external service credentials.
- **R-CONFIG-002** MUST: Configuration access SHALL use the standard process environment property accessor pattern for build-time functions requiring application metadata.
- **R-CONFIG-003** MUST: Configuration access SHALL use the standard process environment property accessor pattern for API client initialization requiring endpoint configuration.
- **R-CONFIG-004** MUST: Configuration access SHALL use the standard process environment property accessor pattern for webhook validation requiring shared secrets.
- **R-CONFIG-005** MUST: Configuration access SHALL use the standard process environment property accessor pattern for any module requiring deployment-environment-specific values.
- **R-CONFIG-006** MUST: Sensitive credentials including tokens and secrets are accessed exclusively through environment variables and never hardcoded in source files.
- **R-CONFIG-007** SHOULD: Create a centralized configuration module that accesses all environment variables in one location, providing typed accessors and validation.
- **R-CONFIG-008** SHOULD: Implement a startup validation function that verifies all required environment variables are present and properly formatted before the application begins processing requests.
- **R-CONFIG-009** SHOULD: Document all required environment variables in deployment documentation, including their purpose, format requirements, and whether they contain sensitive data requiring secret management.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate any startup validation or configuration loading module and verify required environment variables are documented
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -E "(SHOPIFY_STORE_DOMAIN|SHOPIFY_STOREFRONT_ACCESS_TOKEN|SHOPIFY_REVALIDATION_SECRET|SITE_NAME)" | head -20

# Search the codebase for all environment variable access patterns
grep -r "process\.env\[" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | wc -l

# Verify that sensitive values are not hardcoded
grep -r "SHOPIFY_STORE_DOMAIN\s*=\s*['\"]" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | wc -l
grep -r "SHOPIFY_STOREFRONT_ACCESS_TOKEN\s*=\s*['\"]" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | wc -l
grep -r "SHOPIFY_REVALIDATION_SECRET\s*=\s*['\"]" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | wc -l

# Locate the project's testing configuration and verify integration tests for configuration loading
find . -name '*.test.js' -o -name '*.test.ts' -o -name '*.spec.js' -o -name '*.spec.ts' | xargs grep -l "process\.env" 2>/dev/null | head -5

# Search for startup validation or configuration module
find . -type f \( -name 'config.js' -o -name 'config.ts' -o -name 'configuration.js' -o -name 'configuration.ts' -o -name 'env.js' -o -name 'env.ts' \) | head -5
```

**Accept when:**
- All configuration access uses the process environment interface pattern without hardcoded environment-specific values in source files
- Sensitive credentials including tokens and secrets are accessed exclusively through environment variables
- The application provides clear error messages when required environment variables are missing or invalid
- A centralized configuration module exists that validates and provides typed accessors for environment variables
- Startup validation fails fast with clear diagnostics when required environment variables are absent or malformed
- All required environment variables are documented in deployment guides

<enforcement>
Claude Code MUST NOT skip or defer verification. All configuration access patterns must be audited against this rule before code is committed. Static analysis and integration tests must confirm compliance.
</enforcement>