# CodeGraph First

## Goal

Use CodeGraph MCP as the primary source of repository understanding.

## Scope

Use CodeGraph for:

- Symbol lookup and navigation.
- Reference tracing (who calls this method).
- Dependency graph between classes.
- Impact analysis at symbol level.

Do NOT use CodeGraph as a replacement for Code Review Graph.
For change impact, blast radius, and architecture overview, use `06-code-review-graph.md` first.

## Workflow

Before editing code:

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
