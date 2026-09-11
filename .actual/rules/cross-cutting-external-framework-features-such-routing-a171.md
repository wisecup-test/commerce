# Core Library and Component Integration: External Framework Features Such Routing Page

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CORE-001** MUST: External framework features, such as routing and page rendering, MUST be integrated via their designated module imports.

### Verify

```bash
# Discover and execute the project's build command
# e.g., npm run build, yarn build, pnpm build, etc.
# This ensures all dependencies resolve correctly.
discover_and_execute_build_command

# Discover and execute the project's test command
# e.g., npm test, yarn test, pnpm test, etc.
# This validates component and library integrations.
discover_and_execute_test_command

# Discover the project's dependency manifest and inspect it
# e.g., cat package.json, cat Cargo.toml, etc.
# This verifies declared core libraries and components.
discover_and_inspect_dependency_manifest
```

**Accept when:**
- The build process completes without dependency resolution errors.
- All integration tests for components and pages pass.
- The dependency manifest clearly lists the core libraries and components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>