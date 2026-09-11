# Adoption of `next/navigation` and `react` for Client-Side Interactive Components: Use Usestate Useeffect Hooks React Managing

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- R-REACT-001 MUST: Use `useState` and `useEffect` hooks from `react` for managing component-local state and side effects in client-side components.

### Verify

```bash
# Inspect the project's dependency manifest to identify declared `react` and `next/navigation` versions.
# Examine the project's lock file to confirm the exact resolved versions of `react` and `next/navigation`.
# Review client-side component files for the presence of `"use client"` directives and usage of `useState`, `useEffect`, and `searchParams.get()`.
```

**Accept when:**
- The project's dependency resolution for `react` and `next/navigation` is consistent and locked.
- Interactive UI components correctly manage state using `useState` and `useEffect`.
- URL query parameters are accurately read and reflected in client-side components via `next/navigation`.
- Client-side components are appropriately marked with `"use client"`.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>