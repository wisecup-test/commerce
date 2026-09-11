# Core Library and Component Integration: Platform Specific Integrations Such Commerce Encapsulated

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CORE-001** MUST: Platform-specific integrations, such as e-commerce APIs, MUST be encapsulated within dedicated library modules.

### Verify

```bash
# Discover the project's build command and execute it to ensure all dependencies resolve correctly.
# Discover the project's test command and execute it to validate component and library integrations.
# Discover the project's dependency manifest and inspect it for declared core libraries and components.
```

**Accept when:**
- The build process completes without dependency resolution errors.
- All integration tests for components and pages pass.
- The dependency manifest clearly lists the core libraries and components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>