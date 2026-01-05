---
name: logging-design
description:
  Design effective application logging from a software engineering perspective. Use when implementing logging in code,
  choosing log levels, or defining what to log. Focuses on structured logging with Google Cloud Logging standards.
---

# Logging Design for Engineers (Google Cloud)

## Structured Logging (JSON)

Use JSON format for all logs to ensure they are parsed correctly by Google Cloud Logging (formerly Stackdriver).

**Required Fields**:

- `severity`: String (DEBUG, INFO, NOTICE, WARNING, ERROR, CRITICAL, ALERT, EMERGENCY)
- `message`: Human-readable log message (mapped to `jsonPayload.message` or top-level `textPayload` if not JSON)
- `timestamp`: RFC 3339 format (optional, Cloud Logging adds receive time if missing, but generation time is preferred)

**Special Google Cloud Fields**:

- `httpRequest`: Object containing request details (method, URL, status, userAgent, latency). Enables correlating logs with load balancer logs.
- `logging.googleapis.com/trace`: Resource name of the trace associated with the log entry (`projects/[PROJECT_ID]/traces/[TRACE_ID]`).
- `logging.googleapis.com/spanId`: The span ID within the trace.
- `logging.googleapis.com/sourceLocation`: Object with `file`, `line`, `function` for debugging.
- `logging.googleapis.com/labels`: key-value map for indexing and filtering (e.g., `{"env": "prod", "version": "1.2.3"}`).

**Good**: JSON object with severity, message, and labels.

**Bad**: Plain text message with embedded IDs (harder to query).

## Log Levels (Google Cloud Standard)

- **DEBUG**: Debug or trace information. High volume.
- **INFO**: Routine information, such as ongoing status or performance.
- **NOTICE**: Normal but significant events, such as start up, shut down, or a configuration change.
- **WARNING**: Warning events might cause problems.
- **ERROR**: Error events are likely to cause problems.
- **CRITICAL**: Critical events cause more severe problems or outages.
- **ALERT**: A person must take an action immediately.
- **EMERGENCY**: One or more systems are unusable.

**Production Default**: INFO or NOTICE.

## What to Log

- **Application Lifecycle**: Startup configurations, version hashes, clean shutdowns.
- **Request Boundaries**: Ingress and Egress logging using `httpRequest` structure.
- **Business Logic**: Significant state changes (transaction completed, user created).
- **Errors**: Stack traces (formatted to be picked up by Error Reporting), exception messages.
- **Security**: Auth failures, permission denials (exclude sensitive tokens).

## What NOT to Log

- **Secrets**: Passwords, API keys, tokens, PII (Personal Identifiable Information).
- **High Cardinality Data**: Avoid putting dynamic values in `message` string; put them in `jsonPayload`.
- **Duplicate Errors**: Log an exception once at the boundary where it is handled.

## Error Reporting Integration

To trigger Google Cloud Error Reporting:

- Log with severity `ERROR`, `CRITICAL`, `ALERT`, or `EMERGENCY`.
- Include `stack_trace` field in the JSON payload (or `exception` field).
- Or, ensure the `message` contains a stack trace.
- `serviceContext`: (Optional) Object with `service` and `version` to group errors correctly.

## Context and Correlation

- **Trace Correlation**: Populate `logging.googleapis.com/trace` and `logging.googleapis.com/spanId` to view logs inline with traces in Cloud Trace.
- **Service Identity**: handled automatically by the logging agent/environment (resource type `k8s_container`, `gce_instance`, etc.).

## Best Practices

- **Stdout/Stderr**: Write JSON logs directly to standard output/error. The Ops Agent or GKE Fluent Bit will ingest them.
- **Asynchronous Logging**: Don't block the main thread.
- **Sampling**: For high-volume debug logs, sample a percentage to reduce cost.
- **Labels**: Use labels for dimensions you often filter by (tenant ID, region).