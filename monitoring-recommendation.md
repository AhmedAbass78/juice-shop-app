```
## Monitoring Recommendation

### Recommended Stack:
- **Prometheus**: Collect metrics from Kubernetes components
- **Grafana**: Visualize metrics on dashboards
- **Node Exporter**: Collect node-level system metrics
- **kube-state-metrics**: Expose cluster object metrics
- **NGINX Ingress Controller metrics**: For HTTP error rates, response times, etc.
- **Alertmanager**: Send alerts based on Prometheus rules

### Optional Enhancements:
- Loki + Grafana for logs
- Jaeger or OpenTelemetry for tracing
``` 
