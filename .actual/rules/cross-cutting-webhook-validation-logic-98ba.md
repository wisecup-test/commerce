# Source Runtime Configuration from Environment Variables for External Service Integration: Webhook Validation Logic

These rules are ALWAYS ACTIVE for all internal API modules that integrate with external services, webhook handlers that validate incoming requests, configuration initialization code, and error handling logic that reports configuration issues.

### Rules

- **R-WEBHOOK-001** MUST: Webhook validation logic that compares secrets MUST source the expected secret value from the same configuration mechanism used for API credentials.
- **R-WEBHOOK-002** MUST: All required configuration keys MUST be validated at startup and fail immediately with descriptive errors if any are missing or empty, preventing partial initialization.
- **R-WEBHOOK-003** MUST: Related configuration keys MUST use a consistent prefix pattern that identifies the external service, making it clear which keys belong together.
- **R-WEBHOOK-004** MUST: Internal API modules that integrate with external services MUST source credentials and endpoints from environment variables without hardcoded values.
- **R-WEBHOOK-005** SHOULD: Implement a configuration module that encapsulates environment variable access and provides typed accessors, centralizing validation and error handling.
- **R-WEBHOOK-006** MUST: Logging and error handling policies MUST redact configuration values and only expose key names in diagnostic output to prevent credential leakage.

### Verify

```bash
# Discover the project's dependency manifest and identify configuration validation tests
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate integration test suites that exercise API functions with missing configuration
find . -path '*/test*' -o -path '*/spec*' | xargs grep -l 'missing.*config\|environment.*variable' 2>/dev/null | head -5

# Identify static analysis or linting rules that detect direct credential usage
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.golangci.yml' -o -name 'ruleset.xml' | xargs grep -l 'credential\|secret\|hardcoded' 2>/dev/null

# Verify no hardcoded credentials in API modules
grep -r 'password\|token\|secret' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . | grep -v 'process.env\|os.environ\|getenv' | grep -v test | head -10

# Check for environment variable validation at startup
grep -r 'process.env\|os.environ\|getenv' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . | grep -E 'required|validate|throw|raise' | head -10
```

**Accept when:**
- All internal API modules that integrate with external services source credentials and endpoints from environment variables without hardcoded values
- Startup validation confirms required configuration keys are present and fails fast with clear error messages identifying missing keys
- Webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application
- Configuration values are never logged or exposed in error messages in plaintext form
- Related configuration keys follow a consistent service-prefixed naming pattern

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that falls within the defined scope.
</enforcement>