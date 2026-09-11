# Adoption of React Client Components for Interactive UI: Any Module That Contains React Client

These rules are ALWAYS ACTIVE for any module that contains React Client Components.

### Rules

- **R-RCC-001** MUST: Any module that contains React Client Components MUST use the "use client" directive at its top.

### Verify

```bash
# Inspect component files for the "use client" directive at the top of modules requiring interactivity.
# Run the project's build process and observe the generated client-side bundles for components marked "use client".
# Execute the project's UI tests to ensure client-side interactivity functions as expected.
```

**Accept when:**
- The "use client" directive is correctly placed in interactive components.
- Client-side functionality (e.g., state updates, event handling) works without errors.
- The application maintains a good balance of server-side rendering for static content and client-side rendering for dynamic interactions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>