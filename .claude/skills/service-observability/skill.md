---
name: service-observability
description:
  Set up production observability using ELK stack with sidecar pattern in Docker. Use when implementing centralized
  logging and monitoring infrastructure. Focuses on Docker configuration and log shipping.
---

# Service Observability with ELK Stack

## Sidecar Architecture

- Deploy Filebeat container alongside each application service
- Application writes JSON logs to shared volume at `/var/log/app`
- Filebeat reads from shared volume and ships to Elasticsearch
- Kibana connects to Elasticsearch on port 9200
- Services use shared Docker network, remain decoupled from logging infrastructure

## Docker Compose Structure

- Elasticsearch: Single-node cluster, `discovery.type=single-node`, `xpack.security.enabled=false`, expose 9200, set `ES_JAVA_OPTS=-Xms1g -Xmx1g`
- Kibana: Environment `ELASTICSEARCH_HOSTS=http://elasticsearch:9200`, expose 5601
- Application service: Mount named volume to `/var/log/app`, set `SERVICE_NAME` environment variable
- Filebeat sidecar: Mount app's log volume as read-only, mount filebeat.yml as read-only, user root for file access
- Health checks: Elasticsearch curl `/_cluster/health`, Kibana curl `/api/status`
- Depends_on with health conditions: Kibana and Filebeat wait for Elasticsearch healthy
- Named volumes: Elasticsearch data, app logs, Filebeat registry data
- Network: Bridge driver for all services

## Filebeat Configuration

- Input type `log`, paths `/var/log/app/*.log` and `/var/log/app/*.json`
- JSON parsing: `json.keys_under_root: true`, `json.add_error_key: true`, `json.overwrite_keys: true`
- Processors: `decode_json_fields` targeting message field, `add_host_metadata`, `add_docker_metadata`
- Output: `elasticsearch.hosts: ["elasticsearch:9200"]`, index `app-logs-%{+yyyy.MM.dd}`
- Template: `setup.template.name: "app-logs"`, pattern `app-logs-*`, 1 shard, 0 replicas
- ILM: `setup.ilm.enabled: true`, rollover_alias `app-logs`, auto policy creation
- Kibana setup: `setup.kibana.host: "kibana:5601"` for automatic index pattern creation
- Logging: Level info, log to files with 7 day retention

## Application Logging Requirements

- Output JSON to `/var/log/app/app.json` or `/var/log/app/app-YYYY-MM-DD.log`
- Required ECS fields: `@timestamp` (ISO 8601), `log.level`, `message`, `service.name`, `trace.id`
- Context fields: `user.id`, `event.action`, `event.duration`, `http.request.method`, `http.response.status_code`, `url.path`
- Error fields: `error.type`, `error.message`, `error.stack_trace`, `error.code`
- Use structured logging library that supports ECS format natively
- Log asynchronously to avoid blocking application threads
- Rotate log files daily or at 100MB size limit

## Elasticsearch Index Configuration

- Create index template via API: PUT `/_index_template/app-logs-template`
- Index pattern `app-logs-*`, priority 500
- Mappings: `@timestamp` as date, keywords for `log.level`, `service.name`, `trace.id`, `event.action`, `http.request.method`
- Mappings: Long for `event.duration`, `http.response.status_code`
- Mappings: Text for `message`, `error.message`, `error.stack_trace`
- Settings: `number_of_shards: 1`, `number_of_replicas: 0`, `index.mapping.total_fields.limit: 2000`
- Configure ILM policy for retention: rollover at 50GB or 1 day, delete after 30 days

## Verification

- Query Elasticsearch: GET `/_cluster/health` returns green or yellow
- Check indices: GET `/_cat/indices/app-logs-*?v` shows created indices
- Sample logs: GET `/app-logs-*/_search?size=1` returns documents with parsed ECS fields
- Filebeat status: Check container logs for "events have been published" messages
- Field mappings: GET `/app-logs-*/_mapping` confirms ECS field types correct

## Multi-Service Scaling

- Create separate Filebeat sidecar per application service
- Each service uses dedicated named volume for logs
- Each Filebeat config points to corresponding service volume
- All Filebeats output to same Elasticsearch cluster
- Differentiate services via `service.name` field in logs
- Use Elasticsearch alias for cross-service queries

## Production Configuration

- Set Elasticsearch heap to 50% of container memory, max 32GB
- Configure ILM for automatic index lifecycle management and retention
- Enable Elasticsearch cluster monitoring
- Use persistent volumes for Elasticsearch data
- Set application log level to INFO, DEBUG only when troubleshooting
- Implement circuit breaker if logging system unavailable
- Configure Filebeat backpressure and retry settings
- Use separate Elasticsearch clusters for production and non-production environments
- Secure Elasticsearch with TLS and authentication if network-exposed

## Troubleshooting

- No indices created: Check Filebeat connectivity to Elasticsearch, verify app writing logs to volume
- Unparsed JSON fields: Ensure app outputs valid JSON, verify `json.keys_under_root` enabled in Filebeat
- Missing ECS fields: Confirm index template applied before first document, refresh data view
- Filebeat not shipping: Check Elasticsearch health, verify volume mount permissions, review Filebeat logs
- High Elasticsearch memory: Reduce heap size, limit index buffer, decrease field count in mappings
