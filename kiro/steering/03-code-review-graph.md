---
inclusion: fileMatch
fileMatchPattern: "**/*.{java,kt,gradle,gradle.kts,xml,ts,tsx,js,jsx,py,cs,csx,html,cshtml,razor,aspx,ascx,properties,yml,yaml}"
description: code-review-graph MCP usage guide — impact analysis and review workflows
---

## MCP Tools: code-review-graph

## Availability

Apply this steering only when the code-review-graph MCP tools are available in the current session. If unavailable, use normal Git diff, file review, and targeted source inspection.

Use the exact tool names exposed by the current MCP session. Some environments expose names with a `_tool` suffix, for example `detect_changes_tool`, while others may expose shorter names such as `detect_changes`.

### Preferred Scope

Prefer code-review-graph for reviewing pending Git/PR changes, risk scoring, changed-file blast radius, affected execution flows, test coverage mapping, architecture overview, and large refactor planning.

Do not use code-review-graph as the first tool for ordinary symbol navigation, source lookup, method body analysis, or small symbol-level impact checks. Use CodeGraph for those tasks when available.

### Key Tools And Use Cases

| Use case | Preferred tool |
|---|---|
| Initialize or refresh the review graph | `build_or_update_graph_tool` when exposed |
| Minimal repository entry point | `get_minimal_context_tool` when exposed |
| Reviewing incoming git/file changes with risk scoring | `detect_changes_tool` / `detect_changes` |
| Fetching review snippets without loading whole files | `get_review_context_tool` / `get_review_context` |
| Repository-level blast radius of changed files | `get_impact_radius_tool` / `get_impact_radius` |
| Prompt-based pre-commit review workflow | `review_changes` when exposed |
| Repository onboarding | `onboard_developer` when exposed |

Use optional tools such as affected-flow, query-graph, semantic-search, or architecture-overview tools only if they are actually exposed in the current session.

### Core Workflows

#### Code Review Workflow

1. Run the available change-detection tool first to identify high-risk areas.
2. Use review-context or impact-radius tools to inspect only relevant files and flows.
3. Use CodeGraph for deep line-by-line or symbol-level logical analysis after risky files/symbols are isolated.

#### Architecture & Refactoring Impact

1. Use impact-radius or review-context tools before modifying core interfaces/classes or planning a large refactor.
2. Use affected-flow analysis only when the tool is exposed and the change may affect shared execution paths.
3. Do not call a tool unless it appears in the current MCP tool list.

### Boundary With CodeGraph

- Use code-review-graph for code review tasks, pending Git/PR changes, risk scores, changed-file blast radius, review context, architecture overview, and large refactor planning.
- Use CodeGraph for general code navigation, symbol definitions, call hierarchies, method/class logic, and symbol-level impact analysis before small refactors.
- Review workflow priority: identify high-risk changed areas first; then switch to CodeGraph for detailed logical analysis where needed.
