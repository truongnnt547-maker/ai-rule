# CodeGraph First

## Goal

Use CodeGraph MCP as the primary source for symbol-level repository understanding.

## Relationship to `00-tool-routing.md`

`00-tool-routing.md` decides when CodeGraph must be considered. This file defines the detailed CodeGraph workflow once that routing decision is made.

Do not duplicate this whole workflow into always-active rules; keep the always-active gate short.

## Preconditions

Use this rule only when:

- The `codegraph` MCP server is available in the tool list.
- The current project has been initialized/indexed, or index status can be checked safely.

If CodeGraph is unavailable, disabled, or unindexed:

- Do not invent graph results.
- State the reason for skipping CodeGraph.
- Fall back to targeted `search_files` / `read_file` usage.
- Avoid repository-wide scans.

## Scope

Use CodeGraph for:

- Symbol lookup and navigation.
- Caller/callee/reference tracing.
- Execution-flow and bug-path investigation.
- Dependency graph between functions, classes, components, hooks, and modules.
- Symbol-level impact analysis before changing a function, method, class, component, or hook.

Do not use CodeGraph as a replacement for Code Review Graph. For change impact, blast radius, architecture overview, or review scope, use `06-code-review-graph.md` first.

## When To Use

Use CodeGraph before manual source reads/searches for non-trivial tasks involving:

- Understanding how a feature or code path works.
- Finding where a symbol is defined or used.
- Tracing what calls or is called by a symbol.
- Debugging behavior in unfamiliar code.
- Refactoring a known symbol.

For typo, formatting, documentation-only, or single-line local fixes, CodeGraph is optional.

## Tool Selection

Prefer tools in this order:

1. `codegraph_context` — primary tool for broad task context.
2. `codegraph_explore` — inspect related symbols/files when names or code terms are known.
3. `codegraph_search` — quick symbol lookup by name.
4. `codegraph_node` — details for one known symbol.
5. `codegraph_callers` / `codegraph_callees` — direct reference tracing.
6. `codegraph_impact` — symbol-level impact before refactoring.
7. `codegraph_files` / `codegraph_status` — project/index diagnostics only.

## Workflow

Before non-trivial symbol-level edits:

1. Identify the project root and pass it as `projectPath` when supported.
2. Find the target symbol or relevant flow.
3. Inspect callers, callees, and dependencies.
4. Estimate impacted symbols/classes/files.
5. Create a focused modification plan.
6. Only then read or edit source files.

## Fallback Strategy

If a symbol is not found:

1. Verify `projectPath` is correct.
2. Check index status with `codegraph_status`.
3. If the index is stale or the project is unindexed, use targeted `search_files` / `read_file`.
4. Do not fall back to repository-wide scans.

## Project Path

- [CRITICAL] When invoking any CodeGraph tool that supports `projectPath`, always provide `projectPath`.
- [CRITICAL] Obtain the project root from the current workspace, MCP onboarding data, repository metadata, or previous tool results.
- Never guess or invent a project path.
- If the project root is unknown, determine it before using CodeGraph tools.

## Refactoring

Before refactoring a symbol:

- Identify incoming references.
- Identify outgoing dependencies.
- Estimate symbol-level impact.
- Keep the change surgical.

Never refactor blindly.
