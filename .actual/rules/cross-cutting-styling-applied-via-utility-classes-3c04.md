# Adopt Utility-First CSS Styling for UI Components: Styling Applied Via Utility Classes Adhere

These rules are ALWAYS ACTIVE for all UI components and related styling code within the project.

### Rules

- **R-UFCSS-001** MUST: All styling applied via utility classes MUST adhere to the established design system conventions, including support for responsive breakpoints and theme variations.

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