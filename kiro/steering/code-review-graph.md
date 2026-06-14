---
inclusion: fileMatch
fileMatchPattern: "**/*.{java,kt,gradle,xml,ts,tsx,js,jsx,py,cs,csx,html,cshtml,razor,aspx,ascx}"
description: code-review-graph MCP usage guide — impact analysis and review workflows
---
<!-- code-review-graph MCP tools -->
## MCP Tools: code-review-graph

**IMPORTANT:** This project utilizes a knowledge graph. ALWAYS prefer using graph-based tools over structural text scans (Grep/Glob/Read) for understanding git diff / PR review scope, repository-level impact, blast radius by changed files, affected execution flows, test coverage gaps, and architecture. It is faster and token-efficient.

### Key Tools & Use Cases

| Tool | Trigger Condition / Use Case |
| ------ | ---------- |
| `detect_changes` | Reviewing incoming git/file changes (provides risk-scored analysis). |
| `get_review_context` | Fetching source snippets for a review without loading entire files. |
| `get_impact_radius` | Analyzing repository-level blast radius of a proposed change across changed files. |
| `get_affected_flows` | Tracing which business execution paths/flows are impacted. |
| `query_graph` | Finding structural relationships using relations like `callers_of`, `callees_of`, `imports_of`, or `tests_for`. |
| `semantic_search_nodes` | Locating components by architectural keywords **only** if `codegraph_search` fails. |
| `get_architecture_overview` | Getting a high-level birds-eye view of the codebase structure. |

---

### Core Workflows

#### 1. Code Review Workflow
* Run `detect_changes` to identify high-risk spots.
* Use `get_affected_flows` to map the runtime impact.
* Query the graph via `query_graph` (e.g., finding `tests_for`) to verify test coverage for modified areas.

#### 2. Architecture & Refactoring Impact
* Run `get_impact_radius` before modifying core interfaces/classes or planning a large refactor.
* Use `get_affected_flows` when the change may impact shared execution paths.
* Use `refactor_tool` to map renames or clean up dead code safely.

### Boundary with codegraph (STRICT HIERARCHY)
- **Use `code-review-graph` ONLY for:** Code review tasks, analyzing pending Git/PR changes, assessing risk scores (`detect_changes`), analyzing repository-level blast radius across changed files (`get_impact_radius`), mapping impacted execution flows (`get_affected_flows`), locating test coverage (`tests_for`), architecture overview, and large refactor planning.
- **DO NOT use `code-review-graph` for:** General code navigation, searching symbol definitions, tracing call hierarchies from scratch, or symbol-level impact analysis before a small refactor. Delegate those strictly to `codegraph`.
- **Review Workflow Priority:** Always run `detect_changes` first to identify high-risk areas. Once the specific problematic files or symbols are isolated, switch to `codegraph` tools if you need deep, line-by-line logical analysis.
