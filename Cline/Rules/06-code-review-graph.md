---
paths:
  - "src/main/java/**"
  - "src/test/java/**"
  - "**/*.java"
  - "src/components/**"
  - "src/pages/**"
  - "src/hooks/**"
  - "src/features/**"
  - "src/layouts/**"
  - "src/utils/**"
  - "src/services/**"
  - "src/store/**"
  - "src/types/**"
  - "**/*.tsx"
  - "**/*.jsx"
---

# Code Review Graph First

## Goal

Use Code Review Graph MCP to understand repository structure, git diff / PR review scope, repository-level impact, blast radius by changed files, and large refactor planning before reading source files.

## Boundary with CodeGraph

Use **Code Review Graph** for: git diff / PR review, risk scoring, blast radius by changed files, affected execution flows, test coverage gaps, architecture overview, and large refactor planning.
Use **CodeGraph** for: symbol lookup, source exploration, caller/callee tracing, execution flow, method/class logic, and symbol-level impact before a small refactor.

Do NOT use `semantic_search_nodes_tool` if `codegraph_search` already located the symbol — that is redundant.

## Preconditions

Use this rule only when the `code-review-graph` MCP server is available in the tool list.

If Code Review Graph is unavailable, disabled, or uninitialized:

- Do not invent graph results.
- State the reason for skipping Code Review Graph.
- Fall back to targeted `search_files` / `read_file` usage.
- Avoid repository-wide scans.

## When To Use

Use Code Review Graph before manual source reads/searches for non-trivial tasks involving:

- Refactoring existing code.
- Reviewing git diff / PR changes.
- Investigating large or unfamiliar repositories.
- Estimating repository-level impact or blast radius across changed files.
- Finding affected files, modules, services, components, or tests.
- Understanding which execution paths are impacted by a change.
- Planning a large refactor.

For simple syntax fixes, small local edits, or symbol-level navigation, Code Review Graph is optional.

## Tool Selection

| Tool | Use when |
|---|---|
| `get_minimal_context_tool` | First call for any non-trivial review/refactor/architecture task |
| `detect_changes_tool` | Reviewing git diff / PR changes — gives risk-scored analysis |
| `get_review_context_tool` | Need source snippets for review — token-efficient |
| `get_impact_radius_tool` | Understanding blast radius of a change |
| `get_affected_flows_tool` | Finding which execution paths are impacted by a change |
| `query_graph_tool` | Tracing callers, callees, imports, tests, dependencies by pattern |
| `semantic_search_nodes_tool` | Finding functions/classes by name or keyword (use only if codegraph unavailable) |
| `get_architecture_overview_tool` | Understanding high-level codebase structure |

### `query_graph_tool` patterns

- `callers_of` — who calls this function/method
- `callees_of` — what this function/method calls
- `imports_of` — what this file/module imports
- `tests_for` — which tests cover this symbol or file

Build or update the graph only if it is missing or stale.

## Initial Workflow

For non-trivial repository-level tasks:

1. Run `get_minimal_context_tool` first.
2. Identify changed files, target modules, or refactor targets.
3. Run `get_impact_radius_tool` when targets are known.
4. Run `get_affected_flows_tool` to find impacted execution paths.
5. Run `detect_changes_tool` for existing diffs or after modifications.
6. Only then read files, modify code, or run refactoring.

## Refactoring Workflow

Before refactoring:

1. Run `get_minimal_context_tool`.
2. Identify target files, classes, modules, and likely downstream dependencies.
3. Run `get_impact_radius_tool` when targets are known.
4. Run `get_affected_flows_tool` to understand which flows will be affected.
5. Create a focused extraction or modification plan.
6. Refactor one responsibility at a time.
7. Run `detect_changes_tool` after modifications to validate impact.

Identify:

- Impacted modules.
- Affected classes/components/services/controllers/repositories/DTOs.
- Downstream dependencies.
- Potential regression areas.
- Relevant tests or test coverage gaps (`query_graph_tool` pattern `tests_for`).

## Large Class Refactoring

When refactoring classes larger than 300 lines:

1. Run `get_minimal_context_tool`.
2. Identify responsibilities.
3. Identify impacted classes/modules.
4. Create an extraction plan.
5. Extract one responsibility at a time.
6. Validate impact before continuing.

Never immediately rewrite a large class.

## Context Reduction

Prefer compact graph tools before reading large files:

- `get_minimal_context_tool` first.
- `get_review_context_tool` with minimal detail when source snippets are needed.

Avoid:

- Repository-wide scans.
- Loading many files at once.
- Rereading the same file repeatedly.
