# React `use` Hook for Context-Provided Promises: Ensure That Promises Consumed Use Hook

These rules are ALWAYS ACTIVE for React functional components that require access to asynchronous data provided through React Context, especially within the `components/cart` directory or similar domain-specific areas handling asynchronous data.

### Rules

- **R-USE-CONTEXT-PROMISE-001** SHOULD: ensure that Promises consumed by the `use` hook are stable and do not change unnecessarily to prevent excessive re-renders or re-suspensions.

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