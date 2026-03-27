```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install alloy grafana/alloy -f alloy/alloy-values.yaml -n logging
```