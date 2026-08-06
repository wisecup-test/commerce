# Source Runtime Configuration from Environment Variables for External Service Integration: Error Messages Missing

These rules are ALWAYS ACTIVE for all internal API modules that integrate with external services, webhook handlers that validate incoming requests, configuration initialization code, and error handling logic that reports configuration issues.

### Rules

- **R-ENV-001** SHOULD: Error messages for missing configuration SHOULD identify the specific configuration key without exposing the value.

### Verify

```bash
# Discover the project's dependency manifest and identify configuration validation tests
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate integration test suites that exercise API functions with missing configuration
find . -path '*/test*' -o -path '*/spec*' | grep -E '(config|env|integration)' | head -10

# Identify static analysis or linting rules that detect direct credential usage
grep -r 'hardcoded\|credential\|secret' . --include='*.json' --include='*.yaml' --include='*.yml' 2>/dev/null | head -5

# Verify error messages do not expose configuration values
grep -r 'process\.env\|os\.environ\|getenv' . --include='*.js' --include='*.ts' --include='*.py' | grep -v 'key\|name' | head -10
```

**Accept when:**
- All internal API modules that integrate with external services source credentials and endpoints from environment variables without hardcoded values
- Startup validation confirms required configuration keys are present and fails fast with clear error messages identifying missing keys by name only
- Webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application
- Error messages identify the specific configuration key name without including the actual value or credential data
- Configuration validation tests verify that missing environment variables produce appropriate error messages that name the key without exposing sensitive data

<enforcement>
Clause Code MUST NOT skip or defer verification of error message content for missing configuration keys. All error handling paths MUST be reviewed to confirm configuration values are never exposed in diagnostic output.
</enforcement>