---
name: logging-design
description:
  Design effective application logging from a software engineering perspective. Use when implementing logging in code,
  choosing log levels, or defining what to log. Focuses on structured logging, security, and developer best practices.
---

# Logging Design for Engineers

## Structured Logging

Use structured formats (JSON) with consistent fields. Makes logs machine-readable and searchable. Follow Elastic Common Schema (ECS) field names for observability platform compatibility.

**Every log entry must include**:

- `@timestamp`: ISO 8601 with timezone (ECS standard)
- `log.level`: Severity level (use: trace, debug, info, warn, error, fatal)
- `message`: Human-readable description
- `service.name`: Service identifier
- `trace.id`: Correlation ID for distributed tracing

**Add context as separate fields, not in message string**:

- `user.id`, `session.id`, `organization.id`
- `event.action`, `event.duration` (nanoseconds)
- `error.code`, `error.stack_trace`, `error.type`
- `http.request.method`, `http.response.status_code`, `url.path`

**Good**: `log.info("User login", {user: {id: "123"}, event: {action: "user-login"}, method: "oauth"})`

**Bad**: `log.info("User 123 login via oauth")`

## Log Levels

**TRACE**: Step-by-step execution. Development only. High volume.

**DEBUG**: Detailed diagnostics. Development and troubleshooting only.

**INFO**: Normal operations. Default for production. Application lifecycle, business events.

**WARN**: Unexpected but handled. Degraded functionality, fallbacks, retries.

**ERROR**: Failures that don't crash the service. Caught exceptions, failed operations.

**FATAL**: Unrecoverable errors causing shutdown.

**Production default**: INFO. DEBUG creates performance problems and excessive volume.

## What to Log

**Application lifecycle**: Start/stop, version, configuration changes

**Request boundaries**: HTTP requests (method, path, status, duration), API calls to external services

**Business events**: State transitions (order created, payment processed, user registered)

**Authentication events**: Login success/failure, authorization failures, token operations

**Errors**: Exception type, message, stack trace, request context, user impact

**Performance**: Operations exceeding thresholds, slow queries, timeouts

## What NOT to Log

**Never log**:

- Passwords, API keys, tokens, secrets
- Credit card numbers, SSNs
- Full email addresses or phone numbers (hash if needed)
- Session tokens
- Sensitive business data

**Sanitize before logging**: Request/response bodies, query parameters, headers

## Context and Correlation

**Generate trace ID at entry point**: Propagate through entire request lifecycle. Include in all logs as `trace.id`.

**Propagate context**: Pass `trace.id`, `user.id`, `span.id` through function calls and async operations.

**Log at boundaries**: Service entry/exit, external API calls, database operations. Use `event.action` to identify boundary events.

## Performance

**Log asynchronously**: Never block application threads on logging.

**Use appropriate levels**: INFO in production. DEBUG only during troubleshooting.

**Lazy evaluation**: Defer expensive operations until confirmed log level matches.

**Circuit breaker**: Fail gracefully if logging unavailable. Don't crash the application.

## Error Logging

**Include full context**:

- `error.type`: Exception class name
- `error.message`: Exception message
- `error.stack_trace`: Complete stack trace
- `http.*`: Request details that triggered error
- `user.id`: User context (sanitized)
- `event.action`: What was attempted

**Use error codes**: Set `error.code` with unique identifiers for error categories. Makes searching and monitoring easier.

**Log once per error**: Catch and log at appropriate level. Don't re-log as error bubbles up.

## Security

**Sanitize inputs**: Remove sensitive data before logging. Use allow-list approach.

**Hash PII**: If you need to correlate by user_id or session_id, hash them.

**No secrets in errors**: Stack traces and error messages shouldn't expose credentials or keys.

## Message Design

**Be specific**: "User authentication failed" not "Error in login"

**Use past tense**: "Payment processed" not "Processing payment"

**Add context in fields**: Don't interpolate values into message string

**Action-oriented**: Focus on what happened, not code implementation details
