# Adoption of Internal UI Components and Utility Modules: Before Implementing Any New Feature Modifying

These rules are ALWAYS ACTIVE for frontend application code, specifically within page and layout files (`app/**/*.tsx`) and shared `components/` and `lib/` directories.

### Rules

- **R-ADOPTION-001** MUST: Before implementing any new feature or modifying an existing one that relies on external dependencies, developers MUST discover the ecosystem's lock file and resolve the exact locked version of each dependency.

### Verify

```bash
# Run the project's frontend build process to ensure all component imports are resolved correctly.
# Execute the project's UI tests to confirm component functionality and integration.
# Perform a static analysis scan to identify any direct usage of disallowed external UI libraries.
```

**Accept when:**
- The frontend application builds successfully without errors related to component resolution.
- All automated UI tests pass, demonstrating correct component behavior.
- No new direct dependencies on external UI component libraries are introduced without explicit approval.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>