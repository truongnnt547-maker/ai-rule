# Code Review Graph First

## Goal

Use Code Review Graph MCP to understand repository structure,
change impact, and review scope before reading source files.

## Preconditions

Use this rule only when the `code-review-graph` MCP server is available.

If Code Review Graph is unavailable, disabled, or uninitialized:

- Do not invent graph results.
- Fall back to targeted `search_files` / `read_file` usage.
- Avoid repository-wide scans.

## Scope

Use Code Review Graph for:

- Change impact analysis and blast radius.
- Architecture and module overview.
- Identifying affected services, controllers, repositories, DTOs.
- Developer onboarding to an unfamiliar repository.

Do NOT use Code Review Graph for symbol navigation or reference tracing.
For symbol-level analysis, use CodeGraph (see `02-codegraph-first.md`).

## When To Use

Use Code Review Graph before:

- Refactoring existing code
- Reviewing code changes
- Investigating large repositories
- Understanding unfamiliar modules
- Estimating change impact
- Finding affected services, controllers, repositories, DTOs
- Analyzing blast radius
- Developer onboarding

Do not use it for:

- Simple syntax fixes
- Small local edits
- Reading implementation details
- Symbol-level navigation

Use CodeGraph for symbol navigation and references.

## Initial Workflow

For any non-trivial task:

1. Build or update the graph only if it is missing or stale.
2. Run `get_minimal_context_tool` first.
3. Use `detect_changes_tool` when reviewing existing diffs or after making changes.
4. Use `get_impact_radius_tool` when changed files or refactor targets are known.

If an onboarding prompt or tool is available, use it for unfamiliar repositories.
Otherwise, start with `get_minimal_context_tool` and architecture overview tools.

Only after graph context is sufficient:

- read files
- modify code
- run refactoring

## Refactoring Workflow

Before refactoring:

1. Run `get_minimal_context_tool`.
2. Identify the target files, classes, or modules.
3. Run `get_impact_radius_tool` when targets are known.
4. Run `detect_changes_tool` after modifications or when reviewing an existing diff.

Identify:

- impacted modules
- affected classes
- downstream dependencies
- potential regressions

Only then begin implementation.

## Large Class Refactoring

When refactoring classes larger than 300 lines:

1. get_minimal_context_tool
2. identify responsibilities
3. identify impacted classes
4. create extraction plan
5. refactor one responsibility at a time

Never immediately rewrite a large class.

## Context Reduction

Always prefer:

- get_minimal_context_tool
- get_review_context_tool

before reading large files.

Avoid:

- repository-wide scans
- loading many files at once
- rereading the same file repeatedly

## Change Analysis

When reviewing existing changes:

- Run `detect_changes_tool`.
- Analyze affected files, flows, communities, and test coverage gaps.

Before planned non-trivial modifications:

- Run `get_minimal_context_tool` first.
- Use impact analysis when target files or modules are known.

If impact is high:

- analyze blast radius
- analyze dependent modules
- analyze affected tests

before editing.

## Architecture Discovery

For unfamiliar repositories:

1. Run `get_minimal_context_tool`.
2. Use architecture overview or community tools when available.
3. Use onboarding prompts only if exposed by the MCP server.

Use graph context before opening source files.
