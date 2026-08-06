# Access Environment Variables Through Process Object for Runtime Configuration: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all modules that construct HTTP requests to external services, all components that render dynamic content based on deployment environment, all utility functions that validate or transform configuration values, and all API route handlers that verify request authenticity using secrets.

### Rules

- **R-CONFIG-001** MUST: All runtime configuration values including credentials, API tokens, domain names, and deployment URLs must be accessed through the process object's environment property.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'pnpm-lock.yaml' | head -5

# Search for all references to process.env in the codebase
grep -r 'process\.env' --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' .

# Verify no hardcoded credentials exist in source files
grep -r -i 'password\|secret\|token\|key' --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' . | grep -v 'process\.env' | grep -v node_modules | grep -v '.next'

# Locate validation or startup scripts that check environment variable presence
find . -name '*config*' -o -name '*validate*' -o -name '*startup*' | grep -E '\.(ts|tsx|js)$'

# Identify test suites that verify configuration loading behavior
find . -path '*/test*' -o -path '*/__tests__/*' | grep -E '\.(test|spec)\.(ts|tsx|js)$'
```

**Accept when:**
- All configuration values including credentials are accessed through the process environment property and no hardcoded secrets exist in source files
- Validation logic exists that checks for required environment variables and provides clear error messages when they are missing
- Tests verify that the application handles missing or invalid environment variables appropriately
- All references to configuration use process.env and no alternative configuration mechanisms are present without architectural review approval

<enforcement>
Claude Code MUST NOT skip or defer verification. All configuration access points must be audited before accepting changes that introduce new environment-dependent behavior.
</enforcement>