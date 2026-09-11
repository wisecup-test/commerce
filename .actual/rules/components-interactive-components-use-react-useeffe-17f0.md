# Adopt React Hooks for Client-Side Interactivity: Interactive Components Use React Useeffect Hook

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-HOOKS-001** MUST: Interactive UI components MUST use React's `useEffect` hook for handling side effects.

### Verify

```bash
# Discover and run the project's linting commands.
# Discover and run the project's test suite for UI components.
# Discover and run the project's build commands to ensure successful compilation.
```

**Accept when:**
- Linting passes without warnings or errors related to React hooks usage.
- All relevant unit and integration tests for interactive components pass.
- The application builds successfully, and interactive components function as expected in a browser environment.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>