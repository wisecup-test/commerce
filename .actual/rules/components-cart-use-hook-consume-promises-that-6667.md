# React `use` Hook for Context-Provided Promises: Use Hook Consume Promises That Are

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules
- **R-REACT-USE-001** MUST: Use the `use` hook to consume Promises that are provided via React Context for asynchronous data handling.

### Verify
```bash
# Inspect component files for usage of the `use` hook with Context-provided Promises.
# Run the project's test suite to ensure components consuming Promises from Context behave as expected.
# Review the project's build output for any warnings or errors related to Promise consumption.
```

**Accept when:**
- No direct usage of `useEffect` with `useState` for Promise resolution when `use` hook is applicable.
- Components correctly suspend and resolve when consuming Promises from Context.
- The application's UI remains responsive during asynchronous data loading.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>