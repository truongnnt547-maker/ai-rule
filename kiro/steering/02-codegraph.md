---
inclusion: fileMatch
fileMatchPattern: "**/*.{java,kt,gradle,gradle.kts,xml,ts,tsx,js,jsx,py,cs,csx,html,cshtml,razor,aspx,ascx,properties,yml,yaml}"
description: CodeGraph MCP usage guide — when to use which tool
---

## CodeGraph

## Availability

Apply this steering only when the CodeGraph MCP tools are available in the current session. If unavailable, use normal file/search workflow and keep context targeted.

CodeGraph is a tree-sitter-parsed knowledge graph of symbols, edges, and files. It is best for structural questions that grep cannot answer reliably.

### When To Prefer CodeGraph Over Native Search

Use CodeGraph for **structural** questions: what calls what, what would break, where a symbol is defined, and what a symbol's signature/source is. Use native search/read for **literal text** queries such as string contents, comments, log messages, stale-index files, or files that are already specifically identified.

| Question | Tool |
|---|---|
| "Where is X defined?" / "Find symbol named X" | `codegraph_search` |
| "What calls function Y?" | `codegraph_callers` |
| "What does Y call?" | `codegraph_callees` |
| "How does X reach/become Y?" | `codegraph_search` + `codegraph_callers` / `codegraph_callees` |
| "What would break if I changed Z?" | `codegraph_impact` |
| "Show me Y's signature / source / docstring" | `codegraph_node` |
| "See several related symbols' source at once" | `codegraph_explore` |
| "What files exist under path/" | `codegraph_files` |
| "Is the index healthy?" | `codegraph_status` |

### Rules Of Thumb

- For "how does X work" or architecture questions, use `codegraph_search` first, then one `codegraph_explore` for the relevant symbols.
- For a specific flow, locate both ends with `codegraph_search`, then use `codegraph_callers` / `codegraph_callees` and `codegraph_explore` for involved bodies.
- Do not re-check CodeGraph structural results with grep unless the index reports staleness or the task is a literal-text search.
- Do not loop `codegraph_node` over many symbols; prefer one `codegraph_explore` call for related symbols.
- If a CodeGraph response reports pending sync/stale files, read those specific files for accurate current content. Treat files not listed as stale as authoritative.

### If CodeGraph Is Not Initialized

If the MCP server reports that the project is not initialized, ask before running initialization, for example: `codegraph init -i`.

### Boundary With code-review-graph

- Prefer `codegraph` for core development tasks, code navigation, symbol lookups, source exploration, call hierarchies, method/class logic, and symbol-level impact analysis before small refactors.
- Prefer `code-review-graph` for Git/PR audits, change risk scoring, blast radius by changed files, affected execution flows, test coverage mapping, architecture overview, and large refactor planning.
- If the user asks "How does this feature/system work?" or "What does this break at the symbol level?", use `codegraph` when available.
- If the user asks "Review my current git changes" or "What are the risks of my PR?", use `code-review-graph` when available.

### Boundary With Spring Tools

- Prefer Spring Tools for bean lookups, DI wiring, stereotype/component discovery, REST endpoint mapping, and Spring diagnostics.
- Prefer CodeGraph for internal method logic, call hierarchies inside a class, cross-module structural tracing, and impact analysis after Spring Tools identifies the target bean/class.
