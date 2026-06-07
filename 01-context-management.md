# Context Management

## Goal

Minimize context usage and avoid repeatedly loading large files.

## Rules

- Never load more files than necessary.
- Never load an entire repository.
- Read only the files required for the current task.
- Read only the relevant sections of large files.
- Summarize findings before moving to the next step.
- Reuse previous analysis instead of rereading files.

## Defining Relevant Sections

A section is relevant if it:

- Contains the symbol being modified.
- Is a direct caller or callee of the target symbol.
- Is referenced in the current task description.
- Is part of the call chain leading to the failing behavior.

A section is NOT relevant if it:

- Belongs to an unrelated feature or module.
- Was already summarized in a previous step.
- Can be answered by CodeGraph without reading the file.

## Large Repositories

For repositories larger than 50 classes:

- Load no more than 5 files at a time.
- Prefer symbol lookup and dependency analysis.
- Avoid exploratory file reading.

## Validation

Before requesting additional files:

- Verify whether existing context already contains the information.
- Verify whether CodeGraph can provide the answer.
