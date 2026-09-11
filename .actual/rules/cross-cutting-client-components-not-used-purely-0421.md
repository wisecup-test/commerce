# Adoption of React Client Components for Interactive UI: Client Components Not Used Purely Static

These rules are ALWAYS ACTIVE for any React component requiring client-side interactivity or state, or utilizing browser-specific APIs or event handlers.

### Rules

- **R-ADR-001** SHOULD_NOT: Client Components SHOULD NOT be used for purely static or server-rendered UI parts to maximize server-side rendering benefits.

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