---
name: service-observability
description:
  Set up production observability using Google Cloud Ops Agent and Cloud Logging/Monitoring.
  Focuses on VM/Container configuration and Google Cloud Platform integration.
---

- Architecture
  - Data collection: Google Cloud Ops Agent (Compute Engine) or Fluent Bit (GKE default)
  - Transport: agents send to Cloud Logging API and Cloud Monitoring API
  - Storage: Google Cloud Logging buckets and Monarch (monitoring DB)
  - Visualization: Logs Explorer, Metrics Explorer, Dashboards

- Google Cloud Ops Agent (Compute Engine)
  - Install google-cloud-ops-agent package on Linux/Windows VMs
  - Config file: /etc/google-cloud-ops-agent/config.yaml (Linux)
  - Config file: %ProgramFiles%\Google\Cloud Operations\Ops Agent\config\config.yaml (Windows)
  - Unified agent: combines logging (Fluent Bit) and metrics (OpenTelemetry Collector)

- Logging Configuration (Ops Agent)
  - Receivers: files (tail log files), syslog (system logs), tcp/udp (network streams)
  - Processors: parse_json (structured payloads), parse_regex (extract fields)
  - Service pipelines: connect receivers -> processors -> default_pipeline (Cloud Logging)

- Metrics Configuration (Ops Agent)
  - Receivers: hostmetrics (CPU, memory, disk, network), prometheus (scrape endpoints), otlp (gRPC/HTTP)
  - Service pipelines: connect receivers -> default_pipeline (Cloud Monitoring)

- Application Instrumentation
  - Logs: output structured JSON to stdout/stderr (GKE/COS) or file (plain VMs)
  - Logs: follow logging-design skill for field names (severity, httpRequest, etc.)
  - Metrics: use Google Cloud Client Libraries or OpenTelemetry SDK
  - Metrics: configure OTLP exporter to send to Ops Agent (local) or Google Cloud (requires auth)
  - Tracing: use Cloud Trace libraries or OpenTelemetry with Cloud Trace exporter
  - Tracing: support X-Cloud-Trace-Context header for propagation

- Verification
  - Logs Explorer: check resource filters (VM Instance, Kubernetes Container)
  - Logs Explorer: verify JSON payloads are parsed (expandable objects)
  - Logs Explorer: check severity icons match log level
  - Metrics Explorer: verify agent.googleapis.com metrics (host health)
  - Metrics Explorer: verify custom metrics (workload.googleapis.com)
  - Agent status: Linux sudo service google-cloud-ops-agent status
  - Agent logs: /var/log/google-cloud-ops-agent/subagents/logging-module.log

- Best Practices
  - Rely on agent to attach resource metadata (instance ID, zone, project ID)
  - Exclude debug logs in production via exclude_logs processor
  - Use Log Sinks to export high-volume logs to BigQuery or Cloud Storage
  - Create log-based alerts for error patterns directly in Cloud Logging