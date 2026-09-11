# Adopt Utility-First CSS Framework for UI Styling: Components Apply Styling Utility Classes Assigned

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** MUST: UI components MUST apply styling using utility classes assigned to the `className` attribute.

### Verify

```bash
# Inspect UI component files for `className` attributes containing multiple utility classes.
# Run the project's build process and observe the generated CSS output for utility class definitions.
# Execute the project's UI tests to ensure visual consistency.
```

**Accept when:**
- New UI components are styled exclusively using utility classes.
- Existing UI components adhere to the utility-first styling pattern.
- The project's styling output is consistent with a utility-first framework.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>