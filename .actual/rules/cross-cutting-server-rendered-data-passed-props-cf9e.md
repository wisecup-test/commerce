# Adoption of React Client Components for Interactive UI: Server Rendered Data Passed Passed Props Client

These rules are ALWAYS ACTIVE for any React component requiring client-side interactivity or state, or utilizing browser-specific APIs or event handlers.

### Rules

- **R-RSC-DATA-PASS-001** MAY: Server-rendered data MAY be passed as props to Client Components.

### Verify

```bash
# Inspect component files for the "use client" directive at the top of modules requiring interactivity.
# Example (conceptual, actual command depends on project structure):
# grep -r "^'use client'" src/components/

# Run the project's build process and observe the generated client-side bundles for components marked "use client".
# Example (conceptual):
# npm run build # Then manually inspect build output or use bundle analyzer

# Execute the project's UI tests to ensure client-side interactivity functions as expected.
# Example (conceptual):
# npm test -- --runTestsByPath src/components/client-components/
```

**Accept when:**
- The "use client" directive is correctly placed in interactive components.
- Client-side functionality (e.g., state updates, event handling) works without errors.
- The application maintains a good balance of server-side rendering for static content and client-side rendering for dynamic interactions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>