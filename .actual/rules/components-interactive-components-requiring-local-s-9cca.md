# Client-Side UI Interaction with React Hooks: Interactive Components Requiring Local State Management

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: Interactive UI components requiring local state management MUST utilize the `useState` hook from `react`.

### Verify

```bash
# Inspect component files for the presence of `useState` and `useEffect` imports from the `react` library.
# Verify that interactive client-side components include the `use client` directive at the top of the file.
# Examine the project's dependency lock file to confirm the resolved version of the `react` library.
```

**Accept when:**
- `useState` and `useEffect` are consistently used for local state and side effects in interactive components.
- The `use client` directive is present in all components requiring client-side interactivity.
- The resolved `react` version is documented and consistent with project standards.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>