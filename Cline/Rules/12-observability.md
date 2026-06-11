# Observability & Debugging Workflow

## Goal

Structured pipeline for investigating bugs and errors using Grafana logs (vietcap MCP),
CodeGraph, and Code Review Graph — in that order.

## Tool Boundary

- `vietcap` — query Grafana logs to isolate the error (stack trace, class, method, line).
- `codegraph` — locate the symbol in source after identifying it from logs.
- `code-review-graph` — assess impact before writing the fix.

Never use grep or file reads to find a class mentioned in a stack trace — use `codegraph_search`.

## Token Optimization

Never run unbounded log queries. Always narrow by:

- Short time window (e.g., last 15 minutes), or
- Specific identifier (Trace ID, Correlation ID, Exception class name).

For PROD: enforce maximum 5-minute windows due to high log volume.

## Environment Routing

The `vietcap` tool contains data from multiple environments.
**Always determine the target environment before executing any query.**

| Environment | Use when user mentions |
|---|---|
| **PROD** | "live", "production", "prod", "real users", live trading/gateway issues |
| **UAT** | "uat", "staging", "pre-prod", "client testing" |
| **QC** | "qc", "test environment", "qa", internal testing |

### No-Guessing Rule

If the user's prompt does NOT explicitly mention or heavily imply an environment —
**stop and ask**: *"Which environment should I check the logs for: QC, UAT, or PROD?"*

Never guess the environment. Always inject the environment label explicitly into the query
(e.g., `env="prod"` or `namespace="uat"`).

## Triage-to-Fix Pipeline

When investigating a bug or error, follow this order strictly:

### Step 1 — Isolate the Error (`vietcap`)

- Query Grafana logs for the specific error, stack trace, or event failure.
- Common contexts: Spring Boot stack traces, Axon command/event handler exceptions,
  Kafka deduplication offsets, Flowable DelegateTask failures,
  PostgreSQL Outbox SKIP LOCKED contention, Gateway Client timeouts.
- Extract the exact class name, method, and line number from the logs.

### Step 2 — Locate in Source (`codegraph`)

- Use `codegraph_search` to find the class/method identified in Step 1.
- If the error involves a broken async flow (Kafka mid-process, CMMN lifecycle listener),
  use `codegraph_callers` and `codegraph_callees` to follow the path, then `codegraph_explore` for the involved symbols.
- Do NOT use grep or file reads to find the file mentioned in the stack trace.

### Step 3 — Assess Risk (`code-review-graph`)

- Run `get_impact_radius` on the intended modification point before writing any fix.
- Run `get_affected_flows` to identify execution paths that share the same logic.
- Ensure the fix will not break other components relying on the same shared code.

### Step 4 — Implement & Verify

- Write the fix based on root cause — avoid quick hacks.
- Run `detect_changes` from code-review-graph to review your fix against the original bug context.
- Do NOT run tests automatically — only run if the user explicitly requests it.
