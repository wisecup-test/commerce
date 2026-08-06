# Access Environment Variables Through Process Object for Runtime Configuration: Functions That Construct

These rules are ALWAYS ACTIVE for all modules that construct HTTP requests to external services, all components that render dynamic content based on deployment environment, all utility functions that validate or transform configuration values, and all API route handlers that verify request authenticity using secrets.

### Rules

- **R-ENV-001** MUST: Functions that construct external HTTP requests with authentication headers must retrieve credential values from the process environment at the point of use.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'pnpm-lock.yaml' | head -1

# Locate any validation or startup scripts that check environment variable presence
grep -r 'process\.env' --include='*.ts' --include='*.tsx' --include='*.js' | grep -E '(validate|check|require|assert)' | head -20

# Search the codebase for all references to the process environment property
grep -r 'process\.env' --include='*.ts' --include='*.tsx' --include='*.js' | wc -l

# Verify that no credentials are hardcoded in source files
grep -r -E '(SHOPIFY_STOREFRONT_ACCESS_TOKEN|SHOPIFY_REVALIDATION_SECRET|password|secret|token)\s*[=:]\s*["\x27]' --include='*.ts' --include='*.tsx' --include='*.js' | grep -v 'process\.env' | grep -v '\.example' | grep -v 'test'

# Identify test suites that verify configuration loading behavior
find . -name '*.test.ts' -o -name '*.test.tsx' -o -name '*.spec.ts' | xargs grep -l 'process\.env\|configuration\|environment' 2>/dev/null | head -10
```

**Accept when:**
- All configuration values including credentials are accessed through the process environment property and no hardcoded secrets exist in source files
- Validation logic exists that checks for required environment variables and provides clear error messages when they are missing
- Tests verify that the application handles missing or invalid environment variables appropriately
- Code review checks confirm that new configuration values are accessed through the process environment
- Static analysis confirms no hardcoded credentials are present in the codebase

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be verified before accepting code changes.
</enforcement>