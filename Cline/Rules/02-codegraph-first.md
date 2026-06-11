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

# CodeGraph First

## Goal

Use CodeGraph MCP as the primary source for symbol-level repository understanding.
Before using `read_file`, `search_files`, or grep for structural questions, check if CodeGraph can answer it faster.

## Boundary with Code Review Graph

Use **CodeGraph** for: navigation, symbol lookup, call tracing, execution flow, bug paths.
Use **Code Review Graph** for: change impact, blast radius, architecture overview, review scope.

When in doubt: CodeGraph first to locate → Code Review Graph to assess impact.

Do NOT use `semantic_search_nodes` (code-review-graph) if `codegraph_search` already located the symbol.

## Preconditions

Use this rule only when:

- The `codegraph` MCP server is available in the tool list.
- The current project has been initialized/indexed, or index status can be checked safely.

If CodeGraph is unavailable, disabled, or unindexed:

- Do not invent graph results.
- State the reason for skipping CodeGraph.
- Fall back to targeted `search_files` / `read_file` usage.
- Avoid repository-wide scans.

## When To Use

Use CodeGraph before manual source reads/searches for non-trivial tasks involving:

- Understanding how a feature or code path works.
- Finding where a symbol is defined or used.
- Tracing what calls or is called by a symbol.
- Tracing how X reaches Y through async/callback/React/JSX dynamic hops.
- Debugging behavior in unfamiliar code.
- Refactoring a known symbol.

For typo, formatting, documentation-only, or single-line local fixes, CodeGraph is optional.

## Tool Selection

| Question | Tool |
|---|---|
| "Where is X defined?" / "Find symbol named X" | `codegraph_search` |
| "What calls Y?" | `codegraph_callers` |
| "What does Y call?" | `codegraph_callees` |
| "How does X reach Y? / trace flow from X to Y" | `codegraph_trace` |
| "What would break if I changed Z?" | `codegraph_impact` |
| "Show Y's signature / source / docstring" | `codegraph_node` |
| "Give me focused context for a task/area" | `codegraph_context` |
| "See several related symbols' source at once" | `codegraph_explore` |
| "What files exist under path/" | `codegraph_files` |
| "Is the index healthy?" | `codegraph_status` |

Prefer tools in this order for common tasks:

1. `codegraph_context` — broad task context first.
2. `codegraph_trace` — for flow questions ("how does X reach Y"), one call returns the whole path including dynamic hops.
3. `codegraph_explore` — inspect several related symbols' source in one call.
4. `codegraph_search` — quick symbol lookup by name.
5. `codegraph_node` — details for one known symbol.
6. `codegraph_callers` / `codegraph_callees` — direct reference tracing.
7. `codegraph_impact` — symbol-level impact before refactoring.
8. `codegraph_files` / `codegraph_status` — diagnostics only.

## Anti-Patterns

- **Don't grep first** when looking up a symbol by name — `codegraph_search` is faster and returns kind + location + signature in one call.
- **Don't chain `codegraph_search` + `codegraph_node`** for context — use `codegraph_context` instead (one call).
- **Don't loop `codegraph_node` over many symbols** — use one `codegraph_explore` call instead; looping re-reads context and costs far more.
- **Don't rebuild a flow path manually** with `codegraph_search` + `codegraph_callers` — use `codegraph_trace` from→to, which returns the whole path including async/callback/JSX hops in one call.
- **Don't re-verify codegraph results with grep** — results come from a full AST parse and are authoritative.

## Index Staleness

When a codegraph response starts with `"⚠️ Some files referenced below were edited since the last index sync…"`:

- Read those specific listed files directly for accurate content.
- Files NOT in the banner are fresh — trust codegraph for them.
- Run `codegraph_status` to see all pending files under "Pending sync".
- Do not assume all results are stale — only the listed files are pending.

## Workflow

Before non-trivial symbol-level edits:

1. Identify the project root and pass it as `projectPath` when supported.
2. Find the target symbol or relevant flow (`codegraph_context` or `codegraph_trace`).
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

- [CRITICAL] Always provide `projectPath` when the tool supports it.
- [CRITICAL] Obtain from current workspace, MCP onboarding data, repository metadata, or previous tool results.
- Never guess or invent a project path.

## Refactoring

Before refactoring a symbol:

- Identify incoming references.
- Identify outgoing dependencies.
- Estimate symbol-level impact.
- Keep the change surgical.

Never refactor blindly.
