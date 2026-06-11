# Code Review Graph First

## Goal

Use Code Review Graph MCP to understand repository structure, change impact, blast radius, and review scope before reading source files.

## Relationship to `00-tool-routing.md`

`00-tool-routing.md` decides when Code Review Graph must be considered. This file defines the detailed Code Review Graph workflow once that routing decision is made.

Do not duplicate this whole workflow into always-active rules; keep the always-active gate short.

## Preconditions

Use this rule only when the `code-review-graph` MCP server is available in the tool list.

If Code Review Graph is unavailable, disabled, or uninitialized:

- Do not invent graph results.
- State the reason for skipping Code Review Graph.
- Fall back to targeted `search_files` / `read_file` usage.
- Avoid repository-wide scans.

## Scope

Use Code Review Graph for:

- Change impact analysis and blast radius.
- Architecture and module overview.
- Review scope for diffs or local changes.
- Refactor planning across files/modules.
- Identifying affected services, controllers, repositories, DTOs, components, tests, and downstream dependencies.
- Developer onboarding to an unfamiliar repository.

Do not use Code Review Graph for symbol navigation, caller/callee lookup, or reference tracing. Use CodeGraph for symbol-level analysis.

## When To Use

Use Code Review Graph before manual source reads/searches for non-trivial tasks involving:

- Refactoring existing code.
- Reviewing code changes.
- Investigating large or unfamiliar repositories.
- Understanding unfamiliar modules.
- Estimating change impact.
- Analyzing architecture, coupling, or blast radius.
- Finding affected files, modules, services, components, or tests.

For simple syntax fixes, small local edits, documentation-only changes, or symbol-level navigation, Code Review Graph is optional unless impact scope is unclear.

## Tool Selection

Prefer tools in this order:

1. `get_minimal_context_tool` — first call for non-trivial review/refactor/architecture tasks.
2. `detect_changes_tool` — review existing diffs or validate changes after editing.
3. `get_impact_radius_tool` — analyze known changed files or refactor targets.
4. `get_review_context_tool` — focused review context when source snippets are needed.
5. Architecture/community/flow tools — only when the task specifically requires that view.

Build or update the graph only if it is missing or stale.

## Initial Workflow

For non-trivial repository-level tasks:

1. Run `get_minimal_context_tool` first.
2. Identify changed files, target modules, or refactor targets.
3. Run `get_impact_radius_tool` when targets are known.
4. Run `detect_changes_tool` for existing diffs or after modifications.
5. Only then read files, modify code, or run refactoring.

## Refactoring Workflow

Before refactoring:

1. Run `get_minimal_context_tool`.
2. Identify target files, classes, modules, and likely downstream dependencies.
3. Run `get_impact_radius_tool` when targets are known.
4. Create a focused extraction or modification plan.
5. Refactor one responsibility at a time.
6. Run `detect_changes_tool` after modifications when reviewing impact is useful.

Identify:

- Impacted modules.
- Affected classes/components/services/controllers/repositories/DTOs.
- Downstream dependencies.
- Potential regression areas.
- Relevant tests or test coverage gaps.

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

- `get_minimal_context_tool`.
- `get_review_context_tool` with minimal detail when source snippets are needed.

Avoid:

- Repository-wide scans.
- Loading many files at once.
- Rereading the same file repeatedly.
