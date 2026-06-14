# MCP Configuration

## Goal

Document the MCP (Model Context Protocol) server configuration required by this ruleset,
so the agent and user can verify MCP servers are available before using graph-based tools.

## Precondition

Rules `02-codegraph-first.md` and `06-code-review-graph.md` depend on MCP servers.
These rules MUST NOT be followed if the corresponding MCP servers are not running.

## Configuration File

The MCP server configuration lives in a JSON file managed by the Cline/Claude Dev extension:

```
%APPDATA%\Code\User\globalStorage\saoudrizwan.claude-dev\settings\cline_mcp_settings.json
```

On Windows (the current environment):

```
C:\Users\<user>\AppData\Roaming\Code\User\globalStorage\saoudrizwan.claude-dev\settings\cline_mcp_settings.json
```

## Required Servers

The following MCP servers are required by this ruleset:

| Server Name         | Command                          | Used By                    | Required |
|---------------------|----------------------------------|----------------------------|----------|
| `code-review-graph` | `code-review-graph mcp --auto-watch` | `06-code-review-graph.md` | Yes      |
| `codegraph`         | `codegraph serve --mcp`          | `02-codegraph-first.md`    | Yes      |

### Tools Exposed by Each Server

**code-review-graph** (used in rule 06):
- `build_or_update_graph_tool` — Initialise or update the code knowledge graph
- `get_minimal_context_tool` — Ultra-compact entry point (~100 tokens)
- `detect_changes_tool` — Risk-scored change detection
- `get_review_context_tool` — Token-efficient review context
- `get_impact_radius_tool` — Blast radius analysis
- `get_affected_flows_tool` — Affected execution flow analysis
- `query_graph_tool` — Relationship queries such as callers, callees, imports, and tests
- `semantic_search_nodes_tool` — Keyword/semantic search when CodeGraph is unavailable
- `get_architecture_overview_tool` — High-level architecture overview

**codegraph** (used in rule 02) — package: `@colbymchenry/codegraph`:
- `codegraph_explore` — Primary tool: natural-language exploration with verbatim source
- `codegraph_search` — Quick symbol lookup by name
- `codegraph_callers` / `codegraph_callees` — Reference tracing
- `codegraph_impact` — Impact analysis before refactoring
- `codegraph_node` — Full symbol details with source code
- `codegraph_files` — Indexed file tree
- `codegraph_status` — Index health and sync status

## Configuration Structure

```json
{
  "mcpServers": {
    "<server-name>": {
      "command": "<executable>",
      "args": ["<arg1>", "<arg2>"],
      "env": {
        "<ENV_VAR>": "<value>"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

| Field         | Description                                                              |
|---------------|--------------------------------------------------------------------------|
| `command`     | The executable to run (must be on `PATH` or absolute path)               |
| `args`        | Command-line arguments passed to the executable                          |
| `env`         | Environment variables (can be empty `{}`)                                |
| `disabled`    | `false` to enable, `true` to disable the server                         |
| `autoApprove` | List of tool names to auto-approve (empty `[]` means no auto-approval)  |

## Verifying MCP Servers Are Available

Before starting a task that requires rules 02 or 06, verify:

1. **Configuration exists**: The file at the path above contains entries for `codegraph` and `code-review-graph`.
2. **Servers are not disabled**: The `disabled` field is `false` for both servers.
3. **Commands are on PATH**: Each server's `command` resolves to an installed binary. Verify with:
   - `where codegraph` (Windows) / `which codegraph` (macOS/Linux)
   - `where code-review-graph` (Windows) / `which code-review-graph` (macOS/Linux)
4. **Servers are running**: The available MCP tools (listed at the top of every prompt) include tools from both servers.

If any MCP server is missing or disabled, notify the user and do not attempt to use tools from that server.

## Installation

### codegraph (`@colbymchenry/codegraph`)

```bash
# Recommended: global npm install (Node.js required)
npm install -g @colbymchenry/codegraph

# Alternative: no Node.js required (macOS/Linux)
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh

# Alternative: no Node.js required (Windows PowerShell)
irm https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.ps1 | iex
```

> Do NOT use `cargo install codegraph` — that is a different, unrelated Rust project.

After install, initialize each project:

```bash
cd your-project
codegraph init -i
```

Then add to Cline MCP settings (`cline_mcp_settings.json`):

```json
{
  "mcpServers": {
    "codegraph": {
      "command": "codegraph",
      "args": ["serve", "--mcp"],
      "env": {},
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

### code-review-graph

```bash
pip install code-review-graph
```

After install, add to Cline MCP settings:

```json
{
  "mcpServers": {
    "code-review-graph": {
      "command": "code-review-graph",
      "args": ["mcp", "--auto-watch"],
      "env": {},
      "disabled": false,
      "autoApprove": []
    }
  }
}
```
