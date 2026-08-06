# Source Runtime Configuration from Environment Variables for External Service Integration: Internal Modules That

These rules are ALWAYS ACTIVE for all internal API modules that integrate with external services requiring authentication credentials and endpoint configuration at runtime.

### Rules

- **R-ENV-001** MUST: Internal API modules that integrate with external services MUST source runtime configuration from environment variables accessed through the process-level configuration interface.
- **R-ENV-002** MUST: Implement startup validation that reads all required configuration keys and fails immediately with descriptive errors if any are missing or empty, preventing partial initialization.
- **R-ENV-003** MUST: Group related configuration keys using a consistent prefix pattern that identifies the external service, making it clear which keys belong together and simplifying documentation.
- **R-ENV-004** SHOULD: Implement a configuration module that encapsulates environment variable access and provides typed accessors, centralizing validation and error handling.
- **R-ENV-005** MUST: Webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application.
- **R-ENV-006** MUST: Implement logging and error handling policies that redact configuration values and only expose key names in diagnostic output to prevent credential leakage.

### Verify

```bash
# Discover the project's dependency manifest and identify configuration validation tests
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate integration test suites that exercise API functions with missing configuration
find . -path '*/test*' -o -path '*/spec*' | xargs grep -l 'environment\|config' 2>/dev/null | head -5

# Identify static analysis or linting rules that detect direct credential usage
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.golangci.yml' -o -name '.rubocop.yml' 2>/dev/null

# Verify no hardcoded credentials in API modules
grep -r 'password\|token\|secret\|key' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . 2>/dev/null | grep -v 'process.env\|os.environ\|ENV\|getenv' | head -10
```

**Accept when:**
- All internal API modules that integrate with external services source credentials and endpoints from environment variables without hardcoded values
- Startup validation confirms required configuration keys are present and fails fast with clear error messages identifying missing keys
- Webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application
- Configuration values are redacted in logs and error messages, exposing only key names in diagnostic output
- Integration tests validate configuration error handling by running with missing environment variables

<enforcement>
Clause Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review approval.
</enforcement>