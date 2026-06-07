# Refactor Strategy

## Goal

Perform safe incremental refactoring.

## Workflow

Phase 1

- Analyze existing design.
- Identify responsibilities.
- Produce extraction plan.

Phase 2

- Extract one responsibility.
- Validate impact.

Phase 3

- Continue with the next extraction.

## Rules

Never perform large refactors in a single step.

Prefer:

- One responsibility at a time.
- One service at a time.
- One module at a time.

## Limits

Per iteration:

- Modify at most 3 files.
- Extract at most 1 responsibility.
- Keep changes focused.

### Exception: Mechanical Changes

The 3-file limit applies to logic and responsibility changes only.

If a change is purely mechanical (rename, reformat, import update, annotation add)
and affects more than 3 files, complete it in one pass.

Mechanical changes must:

- Involve no logic modification.
- Be verifiable by a simple diff (no behavioral difference).
- Be committed separately from logic changes.

## Validation

After each extraction:

- Verify dependencies.
- Verify imports.
- Verify constructor wiring.
- Verify compile impact.

Only then continue.
