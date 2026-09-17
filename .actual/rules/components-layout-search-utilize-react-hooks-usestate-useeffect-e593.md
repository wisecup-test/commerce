# Adopt React for Client-Side Interactive UI Components: Utilize React Hooks Usestate Useeffect Managing

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** MUST: Utilize React hooks (useState, useEffect) for managing component local state and side effects.

### Verify

```bash
# Discover and execute the project's frontend test suite.
# Discover and execute the project's build command.
# Discover and execute the project's linter and type checker.
```

**Accept when:**
- All frontend tests pass successfully.
- The project builds without errors.
- No linting or type-checking errors are reported.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>