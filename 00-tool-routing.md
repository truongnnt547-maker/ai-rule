---
paths:
  - "**/*"
---

# Tool Routing Gate

## Goal

Choose graph tools before manual context gathering while keeping always-active rule context small.

## Mandatory Gate

Before using `read_file`, `search_files`, `list_files`, manual grep, or opening source code for a non-trivial coding task, decide whether a graph tool applies.

Use **Code Review Graph** first when the task involves:

- Reviewing diffs or local changes.
- Refactoring existing code.
- Unfamiliar modules or repositories.
- Architecture, dependencies, blast radius, or impact scope.
- Identifying affected services, controllers, repositories, DTOs, components, or tests.

Entry points:

- `get_minimal_context_tool` for initial repository or change context.
- `detect_changes_tool` for existing diffs or post-change review.
- `get_impact_radius_tool` when changed files or refactor targets are known.

Use **CodeGraph** first when the task involves:

- Locating symbols or files by code meaning.
- Caller, callee, reference, or dependency tracing.
- Understanding execution flow or bug paths.
- Symbol-level impact before changing a function, method, class, component, or hook.

Entry points:

- `codegraph_context` for broad task context.
- `codegraph_explore` for known symbols, files, or code terms.
- `codegraph_search` for quick symbol lookup.

Always pass `projectPath` to CodeGraph tools when the parameter is supported.

## Allowed Skips

Skip graph tools only when one of these is true:

- The task is a typo, formatting, documentation-only, or single-line local fix.
- The required MCP tool is unavailable in the tool list.
- CodeGraph is unindexed and indexing is not appropriate for the task.
- The task does not require source-code context.

If graph tools are skipped before manual code reads or searches, state the exact reason.

## Context Discipline

This file is the always-active routing gate. Keep detailed workflows in `02-codegraph-first.md` and `06-code-review-graph.md`; do not duplicate their full content here.
