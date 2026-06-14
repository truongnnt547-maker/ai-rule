---
inclusion: always
description: Workflow for using the 'vietcap' (Grafana Logs) MCP for debugging
---
<!------------------------------------------------------------------------------------
   Add rules to this file or a short description that will apply across all your workspaces.
   
   Learn about inclusion modes: https://kiro.dev/docs/steering/#inclusion-modes
-------------------------------------------------------------------------------------> 
# Observability & Debugging Workflow

## 1. The 'vietcap' Tool Boundary (Grafana Logs)
You have access to the `vietcap` MCP tool to query Grafana logs. Use this tool EXCLUSIVELY for troubleshooting, analyzing stack traces, or verifying runtime behavior. 

**TOKEN OPTIMIZATION RULE:** Never run unbounded queries. You MUST narrow down the search by specifying a short time window (e.g., last 15 minutes) or filtering by specific identifiers (e.g., Trace ID, Correlation ID, or specific Exception classes) to prevent massive log dumps.

## 2. Environment Routing (QC, UAT, PROD)
The `vietcap` tool contains data from multiple environments. You MUST explicitly determine the target environment before executing ANY query.

### Environment Mapping Rules:
- **PROD (Production):** Use when the user mentions "live", "production", "prod", "real users", or live trading/gateway issues.
- **UAT (Staging):** Use when the user mentions "uat", "staging", "pre-prod", or "client testing".
- **QC (Testing):** Use when the user mentions "qc", "test environment", "qa", or internal testing.

### Strict Execution Constraints:
1. **The "No-Guessing" Rule:** If the user's prompt DOES NOT explicitly mention or heavily imply an environment, **DO NOT GUESS**. You must halt execution and ask the user: *"Which environment should I check the logs for: QC, UAT, or PROD?"*
2. **Query Injection:** Once the environment is identified, you MUST ensure the environment variable/label (e.g., `env="prod"` or `namespace="uat"`) is explicitly injected into your `vietcap` Grafana query syntax.
3. **PROD Caution:** When querying PROD, enforce stricter time boundaries (e.g., maximum 5-minute windows) due to high log volume.

## 3. The Triage-to-Fix Pipeline
When tasked with investigating a bug or an error, strictly follow this execution order:

### Step 1: Isolate the Error (using `vietcap`)
- Query `vietcap` to fetch the specific error log, stack trace, or event failure based on the confirmed environment.
- *Context Examples:* Look for Spring Boot stack traces, Axon command/event handler exceptions, Kafka deduplication offsets, Flowable DelegateTask failures, PostgreSQL Outbox SKIP LOCKED contention, or Gateway Client timeouts.
- Identify the exact Class name, Method, or line number causing the issue from the logs.

### Step 2: Locate in Source (using `codegraph`)
- Take the Class/Method identified in Step 1 and use `codegraph_search` to find its definition.
- DO NOT use `grep` or file reads to find the file mentioned in the stack trace.
- Use `codegraph_callers` and `codegraph_callees` to follow a broken asynchronous flow, then `codegraph_explore` for the involved symbols (e.g., a message consumed from Kafka failing mid-process, or a CMMN lifecycle listener breaking).

### Step 3: Assess Risk (using `code-review-graph`)
- BEFORE writing the fix, run `get_impact_radius` on the intended modification point.
- Ensure that fixing the current bug will not break other components relying on the same shared logic.

### Step 4: Implement & Verify
- Write the fix based on SOLID principles (avoid quick hacks; fix the root cause).
- Run `detect_changes` from the `code-review-graph` tool to review your own fix against the original bug context before completing the task.