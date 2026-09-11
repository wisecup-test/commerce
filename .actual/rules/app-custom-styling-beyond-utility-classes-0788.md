# Adoption of Internal UI Component Library and Utility-First Styling: Custom Styling Beyond Utility Classes Introduced

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** MAY: Custom styling beyond utility classes MAY be introduced only when existing utility classes cannot achieve the required visual design, and MUST be documented.

### Verify

```bash
# Discover and execute the project's UI component storybook or documentation generation script.
# Run the project's visual regression tests.
# Inspect the project's build output for unused CSS classes.
```

**Accept when:**
- All UI components render correctly according to design specifications.
- Visual regression tests pass without unexpected changes.
- No critical CSS issues or unused styles are reported by build tools.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>