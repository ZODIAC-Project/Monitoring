# Monitoring (Ops self-hosted)

Dieses Verzeichnis bringt ein self-hosted Ops-Monitoring (Metriken/Logs/Traces)
und passt als Basis fuer Agent-Monitoring.

## Stack

- OpenTelemetry Collector
- Prometheus (Metrics)
- Grafana (Dashboards)
- Tempo (Traces)
- Loki (Logs)

## Starten

```bash
cd Monitoring
docker compose up -d
```

Grafana: http://localhost:3000 (admin / admin)
Prometheus: http://localhost:9090
Tempo: http://localhost:3200
Loki: http://localhost:3100

## Kubernetes (empfohlen im Cluster)

Der Monitoring-Stack ist in [deployment-mac.yaml](deployment-mac.yaml) integriert und umfasst:

- OTel Collector (OTLP, Prometheus Export)
- Prometheus, Grafana, Tempo, Loki
- Promtail (DaemonSet) fuer Pod-Logs

Start im Cluster:

```bash
kubectl apply -f deployment-mac.yaml
```

Die lokalen Port-Forwards werden von [starter.sh](starter.sh) gesetzt. Aktuelle Ports:

- Grafana: http://localhost:3001
- Prometheus: http://localhost:9090
- Tempo: http://localhost:3200
- Loki: http://localhost:3101
- OTel Collector Metrics: http://localhost:8889/metrics

## Agent Monitoring (Grafana Dashboard)

Ein Dashboard wird automatisch provisioniert: "Agent Monitoring".

Abgedeckte Metriken:

- agent_create_total
- agent_jobs_total
- agent_job_duration_ms_milliseconds_bucket (p95)
- agent_mcp_request_duration_ms_milliseconds_bucket (p95)
- agent_handoff_enqueued_total
- agent_spawned_children_total

Hinweis: Die Histogramme tragen den Suffix `_milliseconds` durch die OTel-Export-Semantik.

## Log-Shipping (Promtail -> Loki)

Promtail laeuft als DaemonSet und shippt Pod-Logs nach Loki. In der
Default-Config wird der Pfad `/var/log/pods/*/*/*.log` gescraped.
In Loki sind mindestens die `filename` Labels verfuegbar.

## Agent-Services an OTel anschliessen

Setze in Agent-API und Worker folgende Umgebungsvariablen:

```
OTEL_EXPORTER_OTLP_ENDPOINT=http://otel-collector:4318
OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf
OTEL_RESOURCE_ATTRIBUTES=service.name=agent-worker,service.version=dev
```

Fuer die Agent-API entsprechend `service.name=agent-api`.

