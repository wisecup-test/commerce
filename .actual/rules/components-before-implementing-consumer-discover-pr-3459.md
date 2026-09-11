# Adopt React Hooks for Client-Side Interactivity: Before Implementing Consumer Discover Project Dependency

These rules are ALWAYS ACTIVE for all client-side interactive UI components.

### Rules

- **R-REACTHOOKS-001** MUST: Before implementing, the consumer MUST discover the project's dependency manifest and lock file to determine the exact resolved version of React and related libraries, and consult its official documentation.

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