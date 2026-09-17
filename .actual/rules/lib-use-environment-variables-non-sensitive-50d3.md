# Runtime Configuration Management via `process.env`: Use Environment Variables Non Sensitive Specific

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MAY: MAY use environment variables for non-sensitive, environment-specific feature flags or toggles.

### Verify

```bash
# Discover the project's build scripts (e.g., in package.json, Makefile, or build.sh)
# Execute build scripts to ensure successful compilation.
# Example: npm run build

# Discover the project's test scripts (e.g., in package.json, Makefile, or test.sh)
# Execute test scripts to ensure all tests pass.
# Example: npm test

# Discover the project's deployment scripts
# Simulate a deployment to verify environment variable injection.
# This might involve running a local server with specific environment variables,
# or using a deployment tool's dry-run feature.
# Example: VERCEL_ENV=production vercel deploy --prebuilt --prod --dry-run
```

**Accept when:**
- Application builds successfully without errors related to configuration.
- All automated tests pass, including those that rely on configured values.
- The application starts and operates correctly in a deployed environment, accessing configured values as expected.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>