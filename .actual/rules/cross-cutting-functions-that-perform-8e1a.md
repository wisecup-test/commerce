# Source Runtime Configuration from Environment Variables for External Service Integration: Functions That Perform

These rules are ALWAYS ACTIVE for all internal API modules that integrate with external services, perform authenticated HTTP requests, handle webhooks, or initialize API client instances.

### Rules

- **R-ENVCONF-001** MUST: API functions that perform authenticated requests MUST retrieve configuration values at module scope or function scope, never as hardcoded literals.
- **R-ENVCONF-002** MUST: Implement startup validation that reads all required configuration keys and fails immediately with descriptive errors if any are missing or empty, preventing partial initialization.
- **R-ENVCONF-003** MUST: Group related configuration keys using a consistent prefix pattern that identifies the external service, making it clear which keys belong together and simplifying documentation.
- **R-ENVCONF-004** SHOULD: Implement a configuration module that encapsulates environment variable access and provides typed accessors, centralizing validation and error handling.
- **R-ENVCONF-005** MUST: Implement logging and error handling policies that redact configuration values and only expose key names in diagnostic output to prevent credential leakage.

### Verify

```bash
# Discover the project's dependency manifest and identify configuration validation tests
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate integration test suites that exercise API functions with missing configuration
find . -path '*/test*' -o -path '*/spec*' | xargs grep -l 'environment\|config\|missing' 2>/dev/null | head -5

# Identify static analysis or linting rules that detect direct credential usage
grep -r 'hardcoded\|credential\|secret' . --include='.eslintrc*' --include='.pylintrc' --include='ruleset*' 2>/dev/null | head -5

# Search for environment variable access patterns in API modules
grep -r 'process\.env\|os\.environ\|ENV\[' . --include='*.js' --include='*.ts' --include='*.py' --include='*.go' 2>/dev/null | grep -v node_modules | head -10

# Verify no hardcoded credentials or endpoints in source
grep -r 'https://.*\.com\|Bearer [A-Za-z0-9]\|api_key.*=' . --include='*.js' --include='*.ts' --include='*.py' --include='*.go' 2>/dev/null | grep -v node_modules | grep -v '.actual' | wc -l
```

**Accept when:**
- All internal API modules that integrate with external services source credentials and endpoints from environment variables without hardcoded values
- Startup validation confirms required configuration keys are present and fails fast with clear error messages identifying missing keys
- Webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application
- Configuration values are never logged or exposed in error messages in plaintext form
- Related configuration keys follow a consistent service-prefixed naming pattern for clear identification and namespace isolation

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that falls within the stated scope. Violations must be identified during code review and remediated before merge.
</enforcement>