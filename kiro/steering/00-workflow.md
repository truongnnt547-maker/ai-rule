---
inclusion: always
description: Workflow rules — control agent behavior during tasks
---

## Workflow Policy

### Test Execution

- Do NOT run tests automatically after completing a task.
- Compile/build checks may run automatically when they are the directly relevant verification step.
- Only run tests when explicitly asked by the user.
- Do NOT verify changes by running the test suite unless instructed.

### Task Completion

- A task is considered done when the requested changes are made and any explicitly requested verification is completed.
- Do NOT add extra steps such as tests, builds, linting, or broad checks unless the user requests them.
- If verification is skipped because it was not requested, state that clearly without claiming tests passed.
