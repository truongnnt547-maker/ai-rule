---
inclusion: always
---
<!------------------------------------------------------------------------------------
   Add rules to this file or a short description that will apply across all your workspaces.
   
   Learn about inclusion modes: https://kiro.dev/docs/steering/#inclusion-modes
-------------------------------------------------------------------------------------> 
# Headroom Context Optimization

## MCP Usage

When a Headroom MCP server is available:

- Prefer using `headroom_compress` before analyzing large non-code content.
- Prefer compressed representations for exploration and summarization.
- Use `headroom_retrieve` only when additional details are required.
- Use `headroom_stats` after major analysis tasks.
- Minimize unnecessary token consumption whenever possible.

## Purpose

Use Headroom to reduce context size and token usage while preserving reasoning quality.

Headroom should primarily be used for logs, reports, search results, repository scans, generated artifacts, and other large non-source-code content.

Preserve original source code whenever implementation details are important.

## When To Use Headroom

Before analyzing large content, use `headroom_compress`.

Apply compression to:

- Application logs
- Stack traces
- Error reports
- Test reports
- Build output
- CI/CD logs
- Kubernetes logs
- Docker logs
- API responses
- OpenAPI specifications
- Large JSON payloads
- Search results
- Repository scans
- Architecture summaries
- Generated documentation
- Generated reports
- Pull request summaries
- Git diffs larger than 500 lines
- Tool outputs containing hundreds of lines
- Any content that appears too large for efficient context usage

## When NOT To Use Headroom

Do not compress:

- Java source code
- Kotlin source code
- TypeScript source code
- JavaScript source code
- Spring Boot configuration files
- YAML files
- SQL migration files
- Infrastructure as Code files
- Source files under 500 lines
- Files currently being modified
- Files currently being reviewed
- Files currently being refactored

When reviewing, debugging, refactoring, or generating code, always prefer the original source.

## Source Code Analysis

For implementation work:

- Read original source code
- Analyze original source code
- Refactor original source code
- Generate patches from original source code
- Review original source code

Never perform detailed code review using compressed source code.

Compression may be used only for:

- Repository summaries
- Search summaries
- File inventories
- Architecture summaries
- Dependency summaries

## Integration With Observability Pipeline

When following the triage-to-fix pipeline (`observability.md`):

1. After `vietcap` returns log output → immediately call `headroom_compress`
2. Use compressed representation to identify the general error area
3. Call `headroom_retrieve` on the relevant stack trace section BEFORE proceeding to Step 2 (codegraph_search)
4. Only use the retrieved full stack trace to identify exact class name, method, and line number
5. Never pass a compressed stack trace to `codegraph_search` — symbol names may be truncated or lost

## Integration With Code Graph Tools

When using codegraph, code-review-graph, repository indexing, symbol search, dependency graph tools, or similar MCP servers:

- Analyze implementation code directly
- Compress only metadata and summaries
- Compress repository-wide search results
- Compress generated reports
- Compress review summaries

Never compress implementation code returned by graph tools.

## Large Repository Analysis

For repository-wide analysis:

1. Gather search results
2. Compress search results
3. Analyze compressed representation
4. Identify relevant files
5. Retrieve relevant files
6. Analyze original source code
7. Produce findings

Prefer targeted retrieval over loading large portions of the repository into context.

## Git Review Workflow

For large pull requests:

1. Compress diff summaries
2. Identify high-risk areas
3. Retrieve impacted files
4. Review original source code
5. Generate findings

Avoid reviewing large raw diffs directly when compressed summaries are sufficient.

## Retrieval Strategy

If compressed content lacks required details:

1. Use `headroom_retrieve`
2. Retrieve only relevant sections
3. Avoid retrieving the full original content
4. Continue analysis with minimal context expansion

Always prefer targeted retrieval.

## Logging And Diagnostics

Always compress before loading into context:

- Application logs
- Kubernetes logs
- Docker logs
- Build logs
- Test execution logs
- Stack traces
- Error dumps

**Stack trace rule:** Compress to reduce context size, but ALWAYS call `headroom_retrieve` on the relevant stack trace section BEFORE drawing any diagnostic conclusion (identifying root cause, class, method, or line number). Never diagnose from a compressed stack trace directly.

Retrieve original sections only when root-cause analysis requires additional detail.

## Verification

After large analysis tasks:

- Call `headroom_stats`
- Check compression effectiveness
- Verify token savings
- Prefer workflows that reduce context size without losing accuracy

## Priority Order

Prefer Headroom for:

1. Logs
2. Search results
3. Repository scans
4. Generated reports
5. JSON payloads
6. OpenAPI specifications
7. Large Git diffs
8. Build output

Prefer original content for:

1. Source code review
2. Bug fixing
3. Refactoring
4. Architecture analysis
5. Security review
6. Code generation
7. Performance optimization

## General Principle

Compress generated information.

Do not compress implementation details.

Preserve accuracy over token reduction whenever there is a trade-off.

Use the smallest context necessary to complete the task while maintaining correctness.
