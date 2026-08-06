# Access Configuration Values Through Environment Variables: Configuration Access Include

These rules are ALWAYS ACTIVE for all server-side request handling code, build-time functions, API client initialization, webhook validation, and any module requiring deployment-environment-specific values.

### Rules

- **R-CONFIG-001** MUST: Access all deployment-specific configuration values (store domain identifiers, access tokens, revalidation secrets, site naming metadata) through the process environment interface without hardcoding environment-specific values in source code.
- **R-CONFIG-002** MUST: Ensure sensitive credentials including API tokens and shared secrets are accessed exclusively through environment variables and never embedded in source files.
- **R-CONFIG-003** MUST: Implement startup validation that checks for required environment variables and fails fast with clear error messages before the application begins processing requests.
- **R-CONFIG-004** MUST: Document all required environment variables in deployment documentation, including their purpose, format requirements, and whether they contain sensitive data requiring secret management.
- **R-CONFIG-005** SHOULD: Create a centralized configuration module that accesses all environment variables in one location, providing typed accessors and validation to reduce duplication and enable consistent error handling.
- **R-CONFIG-006** SHOULD: Implement logging filters that redact environment variable values to prevent sensitive credentials from being exposed through error messages or logs.
- **R-CONFIG-007** SHOULD: Add explicit type validation and parsing for configuration values at access points to prevent subtle bugs from type coercion and undefined handling.
- **R-CONFIG-008** MAY: Configuration access MAY include fallback values or default assignments when appropriate for non-sensitive settings.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate any startup validation or configuration loading module and verify required environment variables are documented
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -E "(SHOPIFY_STORE_DOMAIN|SHOPIFY_STOREFRONT_ACCESS_TOKEN|SHOPIFY_REVALIDATION_SECRET|SITE_NAME)" | head -20

# Search the codebase for all environment variable access patterns
grep -r "process\.env\|getenv\|ENV\[" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" --include="*.py" --include="*.go" | wc -l

# Verify that sensitive values are not hardcoded
grep -r "SHOPIFY_STORE_DOMAIN\|SHOPIFY_STOREFRONT_ACCESS_TOKEN\|SHOPIFY_REVALIDATION_SECRET" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -v "process\.env" | grep -v "//" | grep -v "#"

# Locate the project's testing configuration and verify integration tests for configuration loading
find . -name '*.test.js' -o -name '*.test.ts' -o -name '*.spec.js' -o -name '*.spec.ts' | xargs grep -l "process\.env\|configuration" 2>/dev/null | head -5
```

**Accept when:**
- All configuration access uses the process environment interface pattern without hardcoded environment-specific values in source files
- Sensitive credentials including tokens and secrets are accessed exclusively through environment variables
- The application provides clear error messages when required environment variables are missing or invalid
- A centralized configuration module exists that validates and provides typed accessors for environment variables
- Startup validation is implemented and documented to fail fast with clear diagnostics
- All required environment variables are documented in deployment guides

<enforcement>
Clause MUST NOT skip or defer verification. Code review MUST confirm no sensitive values are hardcoded. Static analysis MUST scan for hardcoded credentials. Integration tests MUST validate configuration loading behavior with missing or malformed environment variables.
</enforcement>