This folder holds the value file for the loki helm chart. 
```bash 
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install loki grafana/loki -f loki/loki-values.yaml -n logging
```