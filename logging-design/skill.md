---
name: logging-design
description:
  Design effective application logging from a software engineering perspective. Use when implementing logging in code,
  choosing log levels, or defining what to log. Focuses on structured logging with Google Cloud Logging standards.
---

- Structured Logging (JSON)
  - Use JSON format for all logs
  - Required: severity (DEBUG, INFO, NOTICE, WARNING, ERROR, CRITICAL, ALERT, EMERGENCY)
  - Required: message (human-readable)
  - Optional: timestamp (RFC 3339 format, auto-added if missing)
  - httpRequest: object with method, URL, status, userAgent, latency
  - logging.googleapis.com/trace: trace resource name for correlation
  - logging.googleapis.com/spanId: span ID within trace
  - logging.googleapis.com/sourceLocation: file, line, function
  - logging.googleapis.com/labels: key-value map for indexing and filtering

- Log Levels
  - DEBUG: debug or trace information, high volume
  - INFO: routine information, status, performance
  - NOTICE: normal but significant events (startup, shutdown, config change)
  - WARNING: events that might cause problems
  - ERROR: events likely to cause problems
  - CRITICAL: severe problems or outages
  - ALERT: immediate action required
  - EMERGENCY: one or more systems unusable
  - Production default: INFO or NOTICE

- What to Log
  - Application lifecycle: startup configs, version hashes, clean shutdowns
  - Request boundaries: ingress and egress using httpRequest structure
  - Business logic: significant state changes (transaction completed, user created)
  - Errors: stack traces, exception messages (formatted for Error Reporting)
  - Security: auth failures, permission denials (exclude sensitive tokens)

- What NOT to Log
  - Secrets: passwords, API keys, tokens, PII
  - High cardinality data in message string (use jsonPayload fields instead)
  - Duplicate errors (log once at handling boundary)

- Error Reporting Integration
  - Log with severity ERROR, CRITICAL, ALERT, or EMERGENCY
  - Include stack_trace or exception field in JSON payload
  - Or ensure message contains stack trace
  - Optional: serviceContext object with service and version

- Context and Correlation
  - Populate logging.googleapis.com/trace and spanId for Cloud Trace correlation
  - Service identity handled automatically by agent (k8s_container, gce_instance, etc.)

- Best Practices
  - Write JSON logs to stdout/stderr for Ops Agent or GKE Fluent Bit ingestion
  - Use asynchronous logging to avoid blocking main thread
  - Sample high-volume debug logs to reduce cost
  - Use labels for frequently filtered dimensions (tenant ID, region)