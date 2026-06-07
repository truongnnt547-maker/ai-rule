# Code Review Graph First

## Goal

Use Code Review Graph MCP to understand repository structure,
change impact, and review scope before reading source files.

## Scope

Use Code Review Graph for:

- Change impact analysis and blast radius.
- Architecture and module overview.
- Identifying affected services, controllers, repositories, DTOs.
- Developer onboarding to an unfamiliar repository.

Do NOT use Code Review Graph for symbol navigation or reference tracing.
For symbol-level analysis, use CodeGraph (see `02-codegraph-first.md`).

## When To Use

Use Code Review Graph before:

- Refactoring existing code
- Reviewing code changes
- Investigating large repositories
- Understanding unfamiliar modules
- Estimating change impact
- Finding affected services, controllers, repositories, DTOs
- Analyzing blast radius
- Developer onboarding

Do not use it for:

- Simple syntax fixes
- Small local edits
- Reading implementation details
- Symbol-level navigation

Use CodeGraph for symbol navigation and references.

## Initial Workflow

For any non-trivial task:

1. onboard_developer
2. detect_changes_tool
3. get_minimal_context_tool

Only after that:

- read files
- modify code
- run refactoring

## Refactoring Workflow

Before refactoring:

1. detect_changes_tool
2. get_minimal_context_tool
3. get_impact_radius_tool

Identify:

- impacted modules
- affected classes
- downstream dependencies
- potential regressions

Only then begin implementation.

## Large Class Refactoring

When refactoring classes larger than 300 lines:

1. get_minimal_context_tool
2. identify responsibilities
3. identify impacted classes
4. create extraction plan
5. refactor one responsibility at a time

Never immediately rewrite a large class.

## Context Reduction

Always prefer:

- get_minimal_context_tool
- get_review_context_tool

before reading large files.

Avoid:

- repository-wide scans
- loading many files at once
- rereading the same file repeatedly

## Change Analysis

Before modifying existing code:

Run detect_changes_tool.

If impact is high:

- analyze blast radius
- analyze dependent modules
- analyze affected tests

before editing.

## Architecture Discovery

For unfamiliar repositories:

1. onboard_developer
2. get_minimal_context_tool

Use these tools before opening source files.
