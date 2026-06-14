---
inclusion: always
description: Workflow for using the `log-query` MCP tool for Grafana/log debugging
---

# Observability & Debugging Workflow

## Availability

Apply this steering only when the `log-query` MCP tool is available in the current session. If unavailable, ask for the relevant logs, stack trace, trace ID, or reproduction details before proceeding.

## 1. The `log-query` Tool Boundary

Use `log-query` for Grafana/log troubleshooting, stack trace analysis, and runtime-behavior verification when logs are needed.

Prefer targeted source/graph tools instead of log queries when the task is purely static code analysis and no runtime evidence is needed.

### Token Optimization Rule

Never run unbounded log queries. Narrow every query with a short time window or a specific identifier such as Trace ID, Correlation ID, order ID, account ID, request path, or exception class.

For PROD, use a maximum 5-minute window unless the user explicitly provides a narrower identifier that makes a longer query safe.

## 2. Environment Routing: QC, UAT, PROD

The `log-query` tool may contain data from multiple environments. Explicitly determine the target environment before executing any query.

### Environment Mapping Rules

- **PROD:** use when the user mentions "live", "production", "prod", "real users", or live trading/gateway issues.
- **UAT:** use when the user mentions "uat", "staging", "pre-prod", or "client testing".
- **QC:** use when the user mentions "qc", "test environment", "qa", or internal testing.

### Strict Execution Constraints

1. If the user's prompt does not explicitly mention or heavily imply an environment, stop and ask: "Which environment should I check the logs for: QC, UAT, or PROD?"
2. Once the environment is identified, explicitly include the environment label in the query, for example `env="prod"` or `namespace="uat"`.
3. For PROD, keep time boundaries strict because of log volume and production sensitivity.

## 3. Triage-To-Fix Pipeline

When investigating a bug or runtime error, follow this order.

### Step 1: Isolate The Error With `log-query`

- Query `log-query` for the specific error log, stack trace, or event failure in the confirmed environment.
- For large log output, use Headroom compression if available, then retrieve the original relevant stack trace section before extracting exact class name, method, or line number.
- Look for Spring Boot stack traces, Axon command/event handler exceptions, Kafka deduplication offsets, Flowable DelegateTask failures, PostgreSQL Outbox SKIP LOCKED contention, Gateway Client timeouts, or other runtime failure signals.
- Identify the exact class name, method, and line number from the original retrieved log or stack trace, not from compressed text.

### Step 2: Locate In Source

- Use Spring Tools first when the failure is clearly about Spring beans, DI wiring, endpoint mappings, profile configuration, or Spring diagnostics.
- Use `codegraph_search` to find the class/method identified in Step 1 when CodeGraph is available.
- Prefer `codegraph_callers` and `codegraph_callees` to follow broken asynchronous flows, then `codegraph_explore` for involved symbols.
- Use native file/search workflow only when MCP tools are unavailable, the index is stale for the target file, or the task is a literal-text lookup.

### Step 3: Assess Risk

- Before writing a fix, use code-review-graph impact/risk tools when available for shared logic, core interfaces/classes, or changes with broad blast radius.
- Keep the risk check proportional; small isolated fixes do not require a large review workflow if the relevant MCP tool is unavailable.

### Step 4: Implement & Verify

- Write the smallest root-cause fix. Do not refactor unrelated code while fixing an incident.
- If code-review-graph is available and a code change was made, use its change-detection/review tool to review the fix against the original bug context.
- Do not run tests automatically unless the user explicitly requests them.
