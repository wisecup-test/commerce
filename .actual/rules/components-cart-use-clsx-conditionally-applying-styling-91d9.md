# Adoption of @heroicons/react/24/outline for Iconography: Use Clsx Conditionally Applying Styling Icon

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HEROICONS-001** MAY: Use clsx for conditionally applying styling to icon components.

### Verify

```bash
# Inspect the project's dependency manifest for the presence of the icon library.
# Example: grep -r "@heroicons/react" package.json yarn.lock pnpm-lock.yaml
# Search the codebase for imports of the icon library within React components.
# Example: grep -r "from '@heroicons/react'" src/components
# Review UI components to confirm icons are rendered using the specified library's components.
# This typically requires manual visual inspection or UI testing.
```

**Accept when:**
- The icon library is listed as a dependency in the project's manifest.
- Icon components from the library are found in relevant UI files.
- Rendered UI displays consistent iconography.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>