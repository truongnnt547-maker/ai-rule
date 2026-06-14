# Cline Rules and Kiro Steering

A collection of reusable Cline rules and Kiro steering guidance for AI-assisted software engineering. These files help AI coding agents produce cleaner, safer, and more maintainable code by defining conventions, workflows, tool boundaries, and constraints.

## Prerequisites

**MCP Server Requirement:** Some Cline rules and Kiro steering files depend on MCP servers such as CodeGraph, Code Review Graph, Spring Tools, log-query, and Headroom. For the Cline MCP baseline, see [`Cline/Rules/00-mcp-configuration.md`](Cline/Rules/00-mcp-configuration.md) for installation and verification instructions. Kiro steering files include their own availability boundaries and fallback behavior.

## Repository Layout

- `Cline/Rules/` — rules for Cline
- `kiro/steering/` — steering for Kiro

## File Index

### Cline Rules

| File | Purpose |
|---|---|
| [`Cline/Rules/00-mcp-configuration.md`](Cline/Rules/00-mcp-configuration.md) | MCP server configuration, verification, and installation |
| [`Cline/Rules/01-context-management.md`](Cline/Rules/01-context-management.md) | Minimize token usage; avoid loading unnecessary files or sections |
| [`Cline/Rules/02-codegraph-first.md`](Cline/Rules/02-codegraph-first.md) | Use CodeGraph MCP as primary tool for symbol lookup, references, and dependency analysis |
| [`Cline/Rules/03-large-file-handling.md`](Cline/Rules/03-large-file-handling.md) | Strategies for reading and refactoring files over 300 lines |
| [`Cline/Rules/04-refactor-strategy.md`](Cline/Rules/04-refactor-strategy.md) | Incremental, safe refactoring — one responsibility at a time |
| [`Cline/Rules/06-code-review-graph.md`](Cline/Rules/06-code-review-graph.md) | Use Code Review Graph MCP for impact analysis, blast radius, and architecture |
| [`Cline/Rules/07-git-discipline.md`](Cline/Rules/07-git-discipline.md) | Clean commit history: format, separation, and checkpoint rules |
| [`Cline/Rules/08-ignore-files.md`](Cline/Rules/08-ignore-files.md) | Files and directories the agent should never read, scan, or modify |
| [`Cline/Rules/09-general-behavior.md`](Cline/Rules/09-general-behavior.md) | Overarching principles: simplicity, surgical changes, goal-driven execution |
| [`Cline/Rules/10-security-and-privacy.md`](Cline/Rules/10-security-and-privacy.md) | Security checks: secrets handling, authn/z, validation, common web vulns, crypto, logging |
| [`Cline/Rules/11-dependency-and-build-discipline.md`](Cline/Rules/11-dependency-and-build-discipline.md) | Safe dependency/build changes: approval, lockfiles, validation, CI/config scope |
| [`Cline/Rules/12-observability.md`](Cline/Rules/12-observability.md) | Observability-first debugging workflow using Grafana logs, CodeGraph, and Code Review Graph |

### Kiro Steering

| File | Purpose |
|---|---|
| [`kiro/steering/00-workflow.md`](kiro/steering/00-workflow.md) | Task workflow rules for verification and completion |
| [`kiro/steering/01-headroom.md`](kiro/steering/01-headroom.md) | Headroom compression guidance for large non-source-code content |
| [`kiro/steering/02-codegraph.md`](kiro/steering/02-codegraph.md) | CodeGraph MCP usage guide and tool-selection heuristics |
| [`kiro/steering/03-code-review-graph.md`](kiro/steering/03-code-review-graph.md) | Code Review Graph MCP usage guide for impact analysis and review workflows |
| [`kiro/steering/04-spring-tools.md`](kiro/steering/04-spring-tools.md) | Spring Tools MCP usage guide for beans, wiring, endpoints, and diagnostics |
| [`kiro/steering/05-log-query-observability.md`](kiro/steering/05-log-query-observability.md) | Observability and debugging workflow using Grafana/log-query |

## Usage

Use the repository layout as-is:

- Cline reads rules from `Cline/Rules/`
- Kiro reads steering from `kiro/steering/`

### Cline Rule Application Order

1. **00-mcp-configuration.md** — Prerequisite: documents MCP server availability and verification
2. **01-context-management.md** — Keep context small while gathering repository context
3. **02-codegraph-first.md**, **06-code-review-graph.md** — Graph-first workflows for symbol understanding and change impact
4. **03-large-file-handling.md**, **04-refactor-strategy.md** — Apply during refactoring tasks
5. **07-git-discipline.md**, **08-ignore-files.md**, **09-general-behavior.md** — Always active
6. **10-security-and-privacy.md**, **11-dependency-and-build-discipline.md** — Scoped security/build checks that complement the graph-first workflow
7. **12-observability.md** — Structured debugging workflow using Grafana logs, CodeGraph, and Code Review Graph

### Kiro Steering Application Summary

1. **00-workflow.md** — Always active task completion and verification policy
2. **01-headroom.md** — Always active context compression policy when Headroom MCP tools are available
3. **02-codegraph.md**, **03-code-review-graph.md** — File-match steering for graph-based code navigation, impact analysis, and review workflows
4. **04-spring-tools.md** — File-match steering for Spring projects and Spring MCP tool selection
5. **05-log-query-observability.md** — Always active observability workflow for Grafana/log-query debugging

## Key Principles

### Context Efficiency
- Never load more files than necessary.
- Read only the relevant sections of large files.
- Summarize findings before moving on.

### MCP-Powered Graph-First Repository Understanding

**Requires MCP servers when available** (see `00-mcp-configuration.md` for the Cline baseline):
- **CodeGraph** (`@colbymchenry/codegraph`): symbol lookup, references, dependency graph, impact analysis at symbol level
- **Code Review Graph** (`code-review-graph`): change impact, blast radius, architecture overview, developer onboarding
- **Spring Tools**: Spring bean wiring, diagnostics, endpoint mappings, and version metadata in Kiro steering
- **log-query** and **Headroom**: targeted observability and compression workflows in Kiro steering

Use the graph-first rules as the primary decision point before manual source reads: `Cline/Rules/02-codegraph-first.md` and `kiro/steering/02-codegraph.md` for symbol understanding; `Cline/Rules/06-code-review-graph.md` and `kiro/steering/03-code-review-graph.md` for impact and review workflows.

### Incremental Refactoring
- Modify at most 3 files per iteration.
- Extract one responsibility at a time.
- Never perform large refactors in a single step.

### Surgical Changes
- Touch only what you must.
- No speculative abstractions.
- Remove imports that your changes made unused; leave pre-existing dead code alone.

### Testing & Validation
- **Java**: each public Service method needs a unit test; name tests `*Test` (unit) or `*IT` (integration).
- **React**: each custom hook needs a test; prefer React Testing Library.
- After each extraction: verify dependencies, imports, and constructor wiring directly; run compile commands automatically when relevant, and run test commands only after user confirmation.

### Git Discipline
- Follow `type(scope): short description` format.
- Never mix refactoring with feature commits.
- Suggest checkpoints after each phase.

### Stack-Specific Conventions

| Stack | Key Rules |
|---|---|
| **Java / Spring Boot** | Constructor injection, no field injection, layered architecture (Controller → Service → Repository), record DTOs |
| **React / npm** | Functional components only, co-locate files by feature, custom hooks for reusable logic, CSS modules |

### Ignored Artifacts

Target directories, build output, lockfiles, IDE config, `.env.local` files — the agent must never read, scan, or modify these. See `08-ignore-files.md` for the complete list.

**Security:** `.env.local` and similar files may contain secrets and must never be read or included in output.

## License

MIT