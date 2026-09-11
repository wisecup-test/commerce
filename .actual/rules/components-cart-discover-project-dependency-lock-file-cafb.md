# Adoption of @heroicons/react/24/outline for Iconography: Discover Project Dependency Lock File Resolve

These rules are ALWAYS ACTIVE for React components requiring vector icons and UI components within the `components/cart` directory.

### Rules

- **R-HEROICONS-001** MUST: Discover the project's dependency lock file and resolve the exact locked version of `@heroicons/react/24/outline` before implementation.

### Verify

```bash
Inspect the project's dependency manifest for the presence of the icon library.
Search the codebase for imports of the icon library within React components.
Review UI components to confirm icons are rendered using the specified library's components.
```

**Accept when:**
- The icon library is listed as a dependency in the project's manifest.
- Icon components from the library are found in relevant UI files.
- Rendered UI displays consistent iconography.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>