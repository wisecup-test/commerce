# Adopt Utility-First CSS Styling for UI Components: Components Utilize Utility Classes Within Their

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST: UI components MUST utilize utility classes within their `className` attributes for defining visual presentation and layout.

### Verify

```bash
# Inspect the project's build configuration to confirm the presence and proper setup of the utility-first CSS processing.
# Run the project's development server and visually inspect UI components to ensure styling is applied as expected across different screen sizes and themes.
# Execute the project's UI snapshot or visual regression tests to detect any unintended styling changes.
```

**Accept when:**
- All UI components render with the expected visual appearance and layout according to the design specifications.
- Responsive behaviors and theme variations (e.g., dark mode) function correctly across the application.
- No custom, unmanaged CSS files are introduced for general component styling.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>