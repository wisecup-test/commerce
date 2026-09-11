# React `use` Hook for Context-Provided Promises: Use Hook Consume Promises That Are

Status: proposed
Date: 2024-07-30
Deciders: Detection Pipeline (automated)

## Context

- The application requires efficient handling of asynchronous data within React components.
- Data, such as cart information, is often fetched asynchronously and needs to be accessible across a component subtree.
- React Context is utilized to provide global or subtree-wide data.
- The `use` hook offers a mechanism to directly consume Promises within components, potentially simplifying data flow and leveraging concurrent rendering features.

## Problem Statement

How to consistently and effectively consume asynchronous data (Promises) that are provided through React Context within functional components, ensuring proper data flow and integration with React's rendering model.

## Decision

1. MUST: MUST use the `use` hook to consume Promises that are provided via React Context for asynchronous data handling.

## Policy Block

- MUST MUST use the `use` hook to consume Promises that are provided via React Context for asynchronous data handling.

In scope:
- React functional components that require access to asynchronous data provided through React Context.
- Components within the `components/cart` directory or similar domain-specific areas handling asynchronous data.

Out of scope:
- Components that do not consume Promises from React Context.
- Data fetching mechanisms that do not involve React Context or the `use` hook.

## Rationale

- The `use` hook provides a direct and idiomatic way to consume Promises within React components, aligning with modern React patterns for asynchronous data.
- Using Context to provide Promises centralizes data fetching concerns and avoids prop drilling.
- This pattern simplifies component logic by allowing direct consumption of asynchronous values, potentially improving readability and maintainability.

## Consequences

Positive:
- Simplified asynchronous data consumption in React components.
- Improved component readability by directly accessing resolved Promise values.
- Potential for better integration with React's concurrent features and Suspense.

Negative:
- Requires careful management of Promise lifecycle and potential error handling.
- Misuse can lead to unexpected re-suspensions or performance issues if Promises are not stable.

## Alternatives

- Using `useEffect` and `useState` to manage Promise resolution. (rejected)
  Rejected because: This approach often involves more boilerplate code and manual state management compared to the direct consumption offered by the `use` hook.
  When valid: For older React versions or when fine-grained control over the Promise lifecycle is required outside of the `use` hook's capabilities.
- Passing resolved data directly as props. (rejected)
  Rejected because: Leads to prop drilling in deeply nested component trees, reducing maintainability and increasing coupling.
  When valid: For simple, shallow component hierarchies where data is only needed by immediate children.

## Risks

- Unstable Promises causing excessive re-suspensions.
  Mitigation: Ensure Promises provided via Context are memoized or created only when necessary.
  Owner: engineering team
- Inadequate error handling for rejected Promises.
  Mitigation: Implement robust error boundaries and fallback UI for components consuming Promises.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Consider using `React.memo` or `useMemo` for components and values that consume Promises to optimize rendering performance.
- Ensure that the Promises provided to Context are created and managed efficiently to avoid unnecessary re-creations.

## Continuation Context


Verify commands:
- Inspect component files for usage of the `use` hook with Context-provided Promises.
- Run the project's test suite to ensure components consuming Promises from Context behave as expected.
- Review the project's build output for any warnings or errors related to Promise consumption.

Accept when:
- No direct usage of `useEffect` with `useState` for Promise resolution when `use` hook is applicable.
- Components correctly suspend and resolve when consuming Promises from Context.
- The application's UI remains responsive during asynchronous data loading.

## Enforcement

- Verified by: Code reviews, automated linting rules, and integration tests.
- Violation handling: Code review comments, automated build failures, and refactoring tasks.
- Exception process: Documented architectural review and approval process for deviations, requiring clear justification and mitigation strategies.