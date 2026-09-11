# Adoption of React Client Components for Interactive UI: Client Components Used Interactive Elements That

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CLIENT-COMP-001** SHOULD: Client Components SHOULD be used for interactive UI elements that require browser APIs, event listeners, or React Hooks like useState or useEffect.

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