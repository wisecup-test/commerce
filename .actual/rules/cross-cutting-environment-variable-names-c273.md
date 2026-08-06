# Access Environment Variables Through Process Object for Runtime Configuration: Environment Variable Names

These rules are ALWAYS ACTIVE for all modules that construct HTTP requests to external services, all components that render dynamic content based on deployment environment, all utility functions that validate or transform configuration values, and all API route handlers that verify request authenticity using secrets.

### Rules

- **R-ENV-001** SHOULD: Environment variable names should follow a consistent naming convention that clearly identifies the service or component they configure.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'pnpm-lock.yaml' \) | head -5

# Locate any validation or startup scripts that check environment variable presence
grep -r "process\.env" --include="*.ts" --include="*.tsx" --include="*.js" | grep -E "(validation|startup|config)" | head -20

# Search the codebase for all references to the process environment property
grep -r "process\.env" --include="*.ts" --include="*.tsx" --include="*.js" | wc -l

# Verify that no credentials are hardcoded in source files
grep -r "(SHOPIFY_STOREFRONT_ACCESS_TOKEN|SHOPIFY_REVALIDATION_SECRET|password|secret|token)\s*=\s*['\"]" --include="*.ts" --include="*.tsx" --include="*.js" | grep -v "process\.env" | head -10

# Identify test suites that verify configuration loading behavior
find . -type f \( -name '*.test.ts' -o -name '*.test.tsx' -o -name '*.spec.ts' -o -name '*.spec.tsx' \) -exec grep -l "process\.env\|config" {} \;
```

**Accept when:**
- All configuration values including credentials are accessed through the process environment property and no hardcoded secrets exist in source files
- Validation logic exists that checks for required environment variables and provides clear error messages when they are missing
- Tests verify that the application handles missing or invalid environment variables appropriately
- Environment variable names follow a consistent naming convention that clearly identifies the service or component they configure

<enforcement>
Claude Code MUST NOT skip or defer verification. All configuration access points must be audited to ensure compliance with this rule before code is committed.
</enforcement>