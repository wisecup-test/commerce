# Access Environment Variables Through Process Object for Runtime Configuration: Sensitive Credentials Such

These rules are ALWAYS ACTIVE for all modules that construct HTTP requests to external services, all components that render dynamic content based on deployment environment, all utility functions that validate or transform configuration values, and all API route handlers that verify request authenticity using secrets.

### Rules

- **R-CRED-001** MUST: Sensitive credentials such as access tokens and revalidation secrets must never be hardcoded in source files or committed to version control.
- **R-CRED-002** MUST: All configuration values including credentials must be accessed through the process environment property.
- **R-CRED-003** MUST: Validation logic must exist that checks for required environment variables and provides clear error messages when they are missing.
- **R-CRED-004** SHOULD: Create a centralized validation module that checks for required environment variables at application startup and exports typed configuration objects.
- **R-CRED-005** SHOULD: Document all required environment variables in a template or example file that developers can copy to set up their local development environment.
- **R-CRED-006** SHOULD: Implement a configuration schema that validates not just the presence but also the format and constraints of environment variable values.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' | head -1

# Locate any validation or startup scripts that check environment variable presence
grep -r 'process\.env' --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' | grep -E '(validation|startup|config)' | head -20

# Search the codebase for all references to the process environment property
grep -r 'process\.env' --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' | wc -l

# Verify that no credentials are hardcoded in source files
grep -r -E '(password|token|secret|key)\s*[=:]\s*["\']' --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' | grep -v node_modules | grep -v '.actual'

# Identify the project's testing framework and locate test suites
grep -r 'describe\|test\|it(' --include='*.test.ts' --include='*.test.tsx' --include='*.spec.ts' --include='*.spec.tsx' | head -5

# Verify configuration loading and validation logic in tests
grep -r 'process\.env' --include='*.test.ts' --include='*.test.tsx' --include='*.spec.ts' --include='*.spec.tsx' | head -10
```

**Accept when:**
- All configuration values including credentials are accessed through the process environment property and no hardcoded secrets exist in source files
- Validation logic exists that checks for required environment variables and provides clear error messages when they are missing
- Tests verify that the application handles missing or invalid environment variables appropriately
- Code review checks confirm that new configuration values are accessed through the process environment
- Static analysis tools scan for hardcoded credentials and flag violations
- Automated tests verify that configuration loading and validation logic works correctly

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests containing hardcoded credentials are rejected and must be revised before merging. Detected credential leaks trigger immediate credential rotation and security incident response.
</enforcement>