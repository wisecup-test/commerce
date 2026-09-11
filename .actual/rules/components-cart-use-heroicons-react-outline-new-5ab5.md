# Adoption of @heroicons/react/24/outline for Iconography: Use Heroicons React Outline New Iconography

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HEROICONS-001** MUST: Use @heroicons/react/24/outline for all new iconography in React components.

### Verify

```bash
# Verify the icon library is listed as a dependency
grep -q "@heroicons/react" package.json || { echo "Error: @heroicons/react not found in package.json"; exit 1; }

# Verify imports of the icon library within React components
grep -r "import { .* } from '@heroicons/react/24/outline';" src/ || { echo "Warning: No imports from @heroicons/react/24/outline found in src/"; }

# Manual verification: Review UI components to confirm icons are rendered using the specified library's components.
```

**Accept when:**
- The icon library is listed as a dependency in the project's manifest.
- Icon components from the library are found in relevant UI files.
- Rendered UI displays consistent iconography.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>