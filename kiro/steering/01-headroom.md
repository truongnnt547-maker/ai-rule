---
inclusion: always
---

# Headroom Context Optimization

## Availability

Apply this steering only when the Headroom MCP tools are available in the current session. If unavailable, use the smallest normal context needed for the task.

## Purpose

Use Headroom to reduce context size for large non-source-code content while preserving enough detail for accurate reasoning.

## Prefer Headroom For

- Application, Kubernetes, Docker, CI/CD, build, and test logs
- Stack traces and error reports
- Large JSON payloads or API responses
- OpenAPI specifications
- Repository scans, generated reports, and generated documentation
- Search results, PR summaries, and Git diffs larger than 500 lines
- Tool outputs containing hundreds of lines

## Do Not Compress Source For Implementation Work

Do not compress content that is being reviewed, debugged, refactored, modified, or used to generate patches, including:

- Java, Kotlin, TypeScript, JavaScript, Python, C#, SQL, YAML, and IaC source/configuration files
- Spring Boot configuration files
- Files currently being modified or reviewed
- Source files under 500 lines

For implementation work, analyze the original source code. Never perform detailed code review or generate code changes from compressed source.

Compression may be used for repository summaries, search summaries, file inventories, architecture summaries, dependency summaries, and other generated metadata.

## Observability Integration

When following the triage-to-fix pipeline in `05-log-query-observability.md`:

1. If `log-query` returns large log output, call `headroom_compress`.
2. Use the compressed representation only to identify the general error area.
3. Call `headroom_retrieve` on the relevant original stack trace section before extracting class name, method, or line number.
4. Never diagnose from a compressed stack trace directly; symbol names, line numbers, and causal frames may be truncated or lost.

## Graph Tool Integration

When using codegraph, code-review-graph, repository indexing, symbol search, dependency graph tools, or similar MCP servers:

- Analyze implementation code directly.
- Compress only metadata, summaries, generated reports, and large repository-wide search results.
- Never compress implementation code returned by graph tools when exact code behavior matters.

## Retrieval Strategy

If compressed content lacks required details:

1. Use `headroom_retrieve`.
2. Retrieve only relevant sections.
3. Avoid retrieving the full original content unless necessary.
4. Continue analysis with minimal context expansion.

## Verification

After major analysis tasks, `headroom_stats` may be used to check compression effectiveness. Do not call it when it adds overhead without improving the task result.

## General Principle

Compress generated information, logs, and large summaries. Preserve original implementation details whenever correctness depends on exact source, stack trace frames, or configuration values.
