This is the Folder holding the value file for the minio helm chart. To install minio with this values file, run the following command in the terminal:

```bash 
helm repo add minio https://charts.min.io/
helm repo update
helm install minio minio/minio -f minio/minio-values.yaml --namespace logging
helm upgrade minio minio/minio -f minio/minio-values.yaml --namespace logging
```
