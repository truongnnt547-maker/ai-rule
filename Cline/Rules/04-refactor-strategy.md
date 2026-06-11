# Refactor Strategy

## Goal

Perform safe incremental refactoring.

## Workflow

Phase 1

- Analyze existing design.
- Identify responsibilities.
- Produce extraction plan.
- Verify: plan is clear and extraction order is defined.

Phase 2

- Extract one responsibility.
- Validate impact.
- Verify: compilation succeeds, dependencies are correct; run tests only after user confirmation.

Phase 3

- Continue with the next extraction.

## Rules

Never perform large refactors in a single step.

Prefer:

- One responsibility at a time.
- One service at a time.
- One module at a time.

Never mix behavior changes with refactoring.

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
- Verify compile impact directly; run test commands only after user confirmation.

Only then continue.

## Stop Conditions

Stop and reassess if:

- Compilation fails after applying a fix.
- Tests fail after extraction.
- Ownership of a responsibility is unclear.
- Impact scope exceeds initial estimate.

Do not continue refactoring until the issue is resolved.
