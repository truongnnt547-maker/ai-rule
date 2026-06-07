# CodeGraph First

## Goal

Use CodeGraph MCP as the primary source of repository understanding.

## Preconditions

Use this rule only when the `codegraph` MCP server is available and the current project has been initialized/indexed.

If CodeGraph is unavailable, disabled, or unindexed:

- Do not invent graph results.
- Fall back to targeted `search_files` / `read_file` usage.
- Avoid repository-wide scans.

## Scope

Use CodeGraph for:

- Symbol lookup and navigation.
- Reference tracing (who calls this method).
- Dependency graph between classes.
- Impact analysis at symbol level.

Do NOT use CodeGraph as a replacement for Code Review Graph.
For change impact, blast radius, and architecture overview, use `06-code-review-graph.md` first.

## When To Use

Use CodeGraph before non-trivial edits, behavior changes, refactoring, unfamiliar-code investigation, and symbol-level dependency analysis.

For trivial typo, formatting, or single-line syntax fixes, CodeGraph is optional.

## Tool Selection

Prefer tools in this order:

1. `codegraph_explore` — first choice for understanding an area or flow.
2. `codegraph_search` — quick symbol lookup by name.
3. `codegraph_node` — full details for one known symbol.
4. `codegraph_callers` / `codegraph_callees` — direct reference tracing.
5. `codegraph_impact` — symbol-level impact before refactoring.
6. `codegraph_files` / `codegraph_status` — project/index diagnostics only.

## Workflow

Before non-trivial edits:

1. Find symbol.
2. Find references.
3. Identify dependencies.
4. Identify impacted classes.
5. Create modification plan.

Only then begin editing.

## Rules

- Prefer CodeGraph over manual file scanning.
- Prefer symbol search over reading directories.
- Prefer reference analysis over repository-wide search.
- Do not read entire files when symbol-level information is sufficient.
- Never read generated, dependency, or build artifacts listed in `08-ignore-files.md`.

### Fallback Strategy

If a symbol is not found:

1. Verify project path is correct.
2. Check index status with `codegraph_status`.
3. If index is stale or project unindexed, use targeted `search_files` or `read_file`.
4. Do not fall back to repository-wide scans.

### Project Path

- [CRITICAL] When invoking any CodeGraph tool that supports `projectPath`, always provide `projectPath`.
- [CRITICAL] Obtain the project root from the current workspace, MCP onboarding data, repository metadata, or previous tool results.
- Never guess or invent a project path.
- If the project root is unknown, determine it before using CodeGraph tools.

## Refactoring

Before refactoring:

- Build dependency graph.
- Identify incoming references.
- Identify outgoing dependencies.
- Estimate impact scope.

Never refactor blindly.
