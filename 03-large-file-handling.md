# Large File Handling

## Definition

A large file is any file exceeding:

- 300 lines for routine changes
- 500 lines for refactoring tasks

## Rules

Do not immediately read an entire large file.

Instead:

1. Identify responsibilities.
2. Locate relevant methods.
3. Read targeted regions only.

## Refactoring

For large classes:

- Produce a responsibility map first.
- Identify extraction candidates.
- Create an extraction plan.
- Refactor incrementally.

## Forbidden

Avoid:

- Reading a 1000+ line file repeatedly.
- Restarting analysis from the beginning after every failure.
- Rewriting the entire file when only part requires changes.

## Recovery

If compilation fails:

1. Read the compiler error message fully.
2. Identify the exact file and line number reported.
3. Read only that method and its direct callers.
4. Do not reload unrelated classes.
5. Apply the fix.
6. Verify compilation succeeds before continuing to the next step.

If the same error recurs after a fix:

- Re-read only the affected method.
- Do not restart analysis from the top of the file.
- Check whether the fix introduced a new import or dependency issue.
