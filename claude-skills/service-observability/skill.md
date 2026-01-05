---
name: service-observability
description:
  Set up production observability using Google Cloud Ops Agent and Cloud Logging/Monitoring.
  Focuses on VM/Container configuration and Google Cloud Platform integration.
---

# Service Observability (Google Cloud)

## Architecture

- **Data Collection**: Google Cloud Ops Agent (for Compute Engine) or Fluent Bit (GKE default) collects logs and metrics.
- **Transport**: Agents send data to Cloud Logging API and Cloud Monitoring API.
- **Storage & Analysis**: Data stored in Google Cloud Logging buckets and Monarch (monitoring DB).
- **Visualization**: Google Cloud Console (Logs Explorer, Metrics Explorer, Dashboards).

## Google Cloud Ops Agent (Compute Engine)

- **Installation**: Install `google-cloud-ops-agent` package on Linux/Windows VMs.
- **Configuration File**: `/etc/google-cloud-ops-agent/config.yaml` (Linux) or `%ProgramFiles%\Google\Cloud Operations\Ops Agent\config\config.yaml` (Windows).
- **Unified Agent**: Combines logging (Fluent Bit based) and metrics (OpenTelemetry Collector based) into one binary.

## Logging Configuration (Ops Agent)

- **Receivers**: Define sources.
    - `files`: Tail log files (e.g., `/var/log/app.log`).
    - `syslog`: Read from system syslog.
    - `tcp` / `udp`: Listen on network ports for JSON/Syslog streams.
- **Processors**: Parse and modify logs.
    - `parse_json`: Parse JSON strings into structured payloads.
    - `parse_regex`: Extract fields from text logs.
- **Service pipelines**: Connect receivers -> processors -> default_pipeline (Cloud Logging).

**Example Config Approach**: Define a `files` receiver for your log path, a `parse_json` processor for timestamp parsing, and link them in a service pipeline.

## Metrics Configuration (Ops Agent)

- **Receivers**:
    - `hostmetrics`: CPU, Memory, Disk, Network (enabled by default).
    - `prometheus`: Scrape Prometheus endpoints.
    - `otlp`: Receive OTLP metrics (gRPC/HTTP).
- **Service pipelines**: Connect receivers -> default_pipeline (Cloud Monitoring).

## Application Instrumentation Requirements

- **Logs**:
    - Output structured JSON to `stdout` / `stderr` (for GKE/Container-Optimized OS) or a specific file (for plain VMs).
    - Follow `logging-design` skill for field names (`severity`, `httpRequest`, etc.).
- **Metrics**:
    - Use Google Cloud Client Libraries or OpenTelemetry SDK.
    - If using OpenTelemetry, configure the OTLP exporter to send to the Ops Agent (local) or directly to Google Cloud (requires auth).
- **Tracing**:
    - Use Cloud Trace libraries or OpenTelemetry with Cloud Trace exporter.
    - Ensure `X-Cloud-Trace-Context` header support for propagation.

## Verification

- **Logs Explorer**:
    - Check "Resource" filters (e.g., VM Instance, Kubernetes Container).
    - Verify JSON payloads are parsed (expandable objects, not just text strings).
    - Check for `severity` icons matching the log level.
- **Metrics Explorer**:
    - Verify `agent.googleapis.com` metrics (host health).
    - Verify custom metrics (e.g., `workload.googleapis.com`).
- **Agent Status**:
    - Linux: `sudo service google-cloud-ops-agent status`.
    - Log file: `/var/log/google-cloud-ops-agent/subagents/logging-module.log`.

## Best Practices

- **Metadata**: Rely on the agent to attach resource metadata (instance ID, zone, project ID). Do not manually log these unless necessary for app logic.
- **Cost Management**:
    - Exclude debug logs in production via agent config `exclude_logs` processor.
    - Use Log Sinks to export high-volume logs to BigQuery or Cloud Storage for long-term retention/analysis instead of keeping them in hot storage.
- **Alerting**: Create Log-based Alerts for specific error patterns (e.g., "Payment Failed") directly in Cloud Logging.