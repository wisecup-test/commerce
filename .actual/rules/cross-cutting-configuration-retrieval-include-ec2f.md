# Source Runtime Configuration from Environment Variables for External Service Integration: Configuration Retrieval Include

These rules are ALWAYS ACTIVE for all internal API modules that integrate with external services, webhook handlers that validate incoming requests, and configuration initialization code that prepares API client instances.

### Rules

- **R-CONFIG-001** SHOULD: Configuration retrieval SHOULD include validation that required keys are present and non-empty before attempting external service calls.

### Verify

```bash
# Discover the project's dependency manifest and identify configuration validation tests
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate integration test suites that exercise API functions with missing configuration
find . -path '*/test*' -o -path '*/spec*' | xargs grep -l 'missing.*config\|environment.*variable' 2>/dev/null | head -5

# Identify static analysis or linting rules that detect direct credential usage
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.golangci.yml' -o -name '.rubocop.yml' 2>/dev/null

# Verify no hardcoded credentials in API modules
grep -r 'password\|token\|secret' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . 2>/dev/null | grep -v 'process.env\|os.environ\|ENV\|getenv' | head -10
```

**Accept when:**
- All internal API modules that integrate with external services source credentials and endpoints from environment variables without hardcoded values
- Startup validation confirms required configuration keys are present and fails fast with clear error messages identifying missing keys
- Webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application
- Integration tests validate configuration error handling by running with missing environment variables
- Static analysis rules detect and prevent hardcoded credentials or endpoint URLs in API modules

<enforcement>
Claude Code MUST NOT skip or defer verification of environment variable sourcing and configuration validation in all internal API modules that integrate with external services.
</enforcement>