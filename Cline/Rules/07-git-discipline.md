# Git Discipline

## Goal

Maintain a clean, traceable commit history during agentic coding sessions.

## Before Large Changes

- Confirm the current branch is not `main` or `master`.
- Summarize the planned changes before executing.
- If unsure of the branch, run `git branch --show-current` before proceeding.

## Before Committing

- Run `git status --short` before staging or committing.
- Review the diff before committing:
  - `git diff` for unstaged changes.
  - `git diff --cached` for staged changes.
- Stage only files directly related to the current task.
- Do not stage unrelated user changes, untracked files, generated files, or ignored artifacts.
- If unrelated changes are present, mention them but leave them untouched.

## Commit Checkpoints

After each refactoring phase, mention a commit checkpoint only in the completion or phase-reporting context.

Suggested checkpoints:

- After completing Phase 1 extraction (one responsibility extracted and verified).
- After completing a mechanical change (rename, reformat, import update).
- After fixing a failing compilation caused by a refactor.
- Before starting the next extraction iteration.

Never accumulate more than one phase of changes without noting the checkpoint in the phase report.

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

## Commit Rules

- Never create a commit unless the user explicitly requests it.
- Never push unless the user explicitly requests it.
- Mention commit checkpoints after logical phases only in completion or phase-reporting context, but do not commit automatically.
- Keep commit subjects concise, imperative, and specific.
- Prefer lowercase scopes, for example `fix(order): validate submit input`.

## Commit Separation Rules

- Never mix refactoring commits with feature commits.
- Never mix logic changes with mechanical changes in the same commit.
- Test additions should be committed together with the code they test,
  or immediately after in a separate `test(...)` commit.

## Forbidden

- Committing directly to `main` or `master` without confirmation.
- Committing with a generic message such as `fix` or `update`.
- Committing multiple unrelated changes in one commit.
- Running destructive Git commands without explicit confirmation, including:
  - `git reset --hard`
  - `git clean -fd`
  - `git checkout -- <file>` when it would discard changes
  - `git rebase`
  - `git push --force` or `git push --force-with-lease`
- Staging all changes with `git add .` when unrelated changes may exist.
- Committing or pushing automatically after making edits.
