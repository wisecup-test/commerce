# Access Configuration Values Through Environment Variables: Sensitive Configuration Values

These rules are ALWAYS ACTIVE for all server-side request handling code, build-time functions, API client initialization, webhook validation, and any module requiring deployment-environment-specific values.

### Rules

- **R-CONFIG-001** MUST: Sensitive configuration values including access tokens, secrets, and credentials SHALL be retrieved exclusively through environment variables.
- **R-CONFIG-002** MUST: All server-side request handling code requiring external service credentials SHALL access configuration through the process environment interface.
- **R-CONFIG-003** MUST: Build-time functions requiring application metadata SHALL access configuration through environment variables.
- **R-CONFIG-004** MUST: API client initialization requiring endpoint configuration SHALL retrieve values from environment variables.
- **R-CONFIG-005** MUST: Webhook validation requiring shared secrets SHALL access secrets through environment variables.
- **R-CONFIG-006** MUST: No sensitive values or environment-specific configuration SHALL be hardcoded in source files.
- **R-CONFIG-007** SHOULD: Create a centralized configuration module that accesses all environment variables in one location, providing typed accessors and validation.
- **R-CONFIG-008** SHOULD: Implement a startup validation function that verifies all required environment variables are present and properly formatted before the application begins processing requests.
- **R-CONFIG-009** SHOULD: Document all required environment variables in deployment documentation, including their purpose, format requirements, and whether they contain sensitive data.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name "package.json" -o -name "requirements.txt" -o -name "go.mod" -o -name "Gemfile" | head -1

# Locate any startup validation or configuration loading module
find . -type f \( -name "*config*" -o -name "*env*" \) -path "*/src/*" -o -path "*/lib/*" | grep -E "\.(js|ts|py|go|rb)$"

# Search the codebase for all environment variable access patterns
grep -r "process\.env" . --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" 2>/dev/null || echo "No process.env found"

# Search for hardcoded credentials or sensitive values
grep -r "SHOPIFY_STORE_DOMAIN\|SHOPIFY_STOREFRONT_ACCESS_TOKEN\|SHOPIFY_REVALIDATION_SECRET\|SITE_NAME" . --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -v "process\.env" | grep -v ".env" 2>/dev/null || echo "No hardcoded credentials found"

# Locate and execute integration tests for configuration loading
find . -type f \( -name "*.test.*" -o -name "*.spec.*" \) | grep -i config | head -5
```

**Accept when:**
- All configuration access uses the process environment interface pattern without hardcoded environment-specific values in source files
- Sensitive credentials including tokens and secrets are accessed exclusively through environment variables
- The application provides clear error messages when required environment variables are missing or invalid
- A centralized configuration module exists that validates and provides typed accessors for environment variables
- Startup validation fails fast with clear diagnostics when required environment variables are absent or malformed
- All required environment variables are documented in deployment guides

<enforcement>
Claude Code MUST NOT skip or defer verification. All configuration access patterns MUST be audited to ensure compliance with R-CONFIG-001 through R-CONFIG-009. Static analysis for hardcoded credentials MUST be performed before accepting any changes.
</enforcement>