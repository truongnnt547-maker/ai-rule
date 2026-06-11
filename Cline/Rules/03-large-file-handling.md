# Large File Handling

## Definition

A large file is any source code file exceeding:

- 300 lines for routine changes
- 500 lines for refactoring tasks

This applies to implementation files (.java, .tsx, .jsx, .rs, .py, etc.), not documentation or configuration files unless they are being refactored or debugged.

## Pre-Read Workflow

Before reading a large file:

1. Use symbol lookup, search, or file outline tools (CodeGraph, search_files, list_code_definition_names) to locate relevant sections.
2. Identify the smallest method, class, or range needed for the task.
3. Read only that targeted range first.
4. Expand context only to direct callers, callees, imports, or adjacent code needed to understand the change.

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
- Extract one responsibility at a time.
- Validate after each extraction (dependencies, imports, and compile impact).
- Never mix cleanup with behavior changes.
- Refactor incrementally.

## Forbidden

Avoid:

- Reading a 1000+ line file repeatedly.
- Restarting analysis from the beginning after every failure.
- Rewriting the entire file when only part requires changes.
- Whole-file rewrites unless the file is generated from a smaller source or the user explicitly requests it.

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

## Exceptions

Reading the whole file is acceptable only when:

- The user explicitly asks for a full-file review.
- The file is only slightly above the threshold (≤350 lines) and structurally simple.
- Search or symbol lookup cannot identify the relevant section.
- The file is documentation and the requested task requires full context.
