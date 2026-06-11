---
inclusion: always
description: Workflow rules — control agent behavior during tasks
---
<!------------------------------------------------------------------------------------
   Add rules to this file or a short description that will apply across all your workspaces.
   
   Learn about inclusion modes: https://kiro.dev/docs/steering/#inclusion-modes
-------------------------------------------------------------------------------------> 
## Workflow Policy

### Test Execution
- Do NOT run tests automatically after completing a task
- Only run tests when explicitly asked by the user
- Do NOT verify changes by running the test suite unless instructed

### Task Completion
- A task is considered done when the code changes are made
- Do NOT add extra steps (test, build, lint) unless the user requests it