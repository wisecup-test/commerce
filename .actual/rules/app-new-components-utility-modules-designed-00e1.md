# Adoption of Internal UI Components and Utility Modules: New Components Utility Modules Designed Reusability

These rules are ALWAYS ACTIVE for all files matching the configured scope, specifically within frontend application code, page and layout files (`app/**/*.tsx`) and shared `components/` and `lib/` directories.

### Rules

- **R-ADOPTION-001** SHOULD: New UI components and utility modules SHOULD be designed for reusability and adhere to existing naming conventions and structural patterns.

### Verify

```bash
Run the project's frontend build process to ensure all component imports are resolved correctly.
Execute the project's UI tests to confirm component functionality and integration.
Perform a static analysis scan to identify any direct usage of disallowed external UI libraries.
```

**Accept when:**
- The frontend application builds successfully without errors related to component resolution.
- All automated UI tests pass, demonstrating correct component behavior.
- No new direct dependencies on external UI component libraries are introduced without explicit approval.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>