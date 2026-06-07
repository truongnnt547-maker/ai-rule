# Git Discipline

## Goal

Maintain a clean, traceable commit history during agentic coding sessions.

## Before Large Changes

- Confirm the current branch is not `main` or `master`.
- Summarize the planned changes before executing.
- If unsure of the branch, run `git branch --show-current` before proceeding.

## Commit Checkpoints

After each refactoring phase, suggest a commit checkpoint.

Suggested checkpoints:

- After completing Phase 1 extraction (one responsibility extracted and verified).
- After completing a mechanical change (rename, reformat, import update).
- After fixing a failing compilation caused by a refactor.
- Before starting the next extraction iteration.

Never accumulate more than one phase of changes without a checkpoint suggestion.

## Commit Message Format

```
type(scope): short description
```

Types:

- `feat` — new feature
- `fix` — bug fix
- `refactor` — code restructuring without behavior change
- `chore` — build, config, dependency changes
- `test` — adding or updating tests

Examples:

```
refactor(order): extract PaymentService from OrderService
test(order): add unit tests for PaymentService
fix(order): correct null check in OrderService.submit
```

## Commit Separation Rules

- Never mix refactoring commits with feature commits.
- Never mix logic changes with mechanical changes in the same commit.
- Test additions should be committed together with the code they test,
  or immediately after in a separate `test(...)` commit.

## Forbidden

- Committing directly to `main` or `master` without confirmation.
- Committing with a generic message such as `fix` or `update`.
- Committing multiple unrelated changes in one commit.
