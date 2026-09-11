# Adoption of @heroicons/react/24/outline for Iconography: Integrate Icons Heroicons React Outline Directly

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HEROICONS-001** SHOULD: Integrate icons from @heroicons/react/24/outline directly as React components within JSX.

### Verify

```bash
# Inspect the project's dependency manifest for the presence of the icon library.
# Search the codebase for imports of the icon library within React components.
# Review UI components to confirm icons are rendered using the specified library's components.
```

**Accept when:**
- The icon library is listed as a dependency in the project's manifest.
- Icon components from the library are found in relevant UI files.
- Rendered UI displays consistent iconography.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>