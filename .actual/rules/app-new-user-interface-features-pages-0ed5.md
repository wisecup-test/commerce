# Adoption of Internal UI Component Library and Utility-First Styling: New User Interface Features Pages Utilize

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: All new user interface features and pages MUST utilize components from the internal UI component library.

### Verify

```bash
Discover and execute the project's UI component storybook or documentation generation script.
Run the project's visual regression tests.
Inspect the project's build output for unused CSS classes.
```

**Accept when:**
- All UI components render correctly according to design specifications.
- Visual regression tests pass without unexpected changes.
- No critical CSS issues or unused styles are reported by build tools.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>