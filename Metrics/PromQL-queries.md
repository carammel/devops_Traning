### Per-container cpu usage rate percentage on 60m:
(sum(rate(container_cpu_usage_seconds_total{container!~"POD|"}[60m])) by (namespace,pod,container)) * 100 ###The above query produces the rate of increase over the last 60 minutes, which lets you see how much computing power the CPU is using. To get the result as a percentage, multiply the query by 100.

### Per-container memory usage in bytes:
- sum(container_memory_usage_bytes{container!~"POD|"}) by (namespace,pod,container)

####Belowed example didn't work on prometheus
### Per-container CPU usage in CPU cores:
- sum(rate(container_cpu_usage_seconds_total{container!~"POD|"}[5m])) by (namespace,pod,container)

### Per-pod memory usage in bytes:
- sum(container_memory_usage_bytes{container!=""}) by (namespace,pod)

### Per-pod CPU usage in CPU cores:
- sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (namespace,pod)

### Per-node memory usage in bytes:
- sum(container_memory_usage_bytes{container!=""}) by (node)

Per-pod CPU usage in CPU cores:
- sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (namespace,pod)

Per-node memory usage in bytes:
- sum(container_memory_usage_bytes{container!=""}) by (node)

Per-node CPU usage in CPU cores:
- sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (node)

####Belowed example didn't work on prometheus and OP
- Per-node memory usage percentage:
100 * (
  sum(container_memory_usage_bytes{container!=""}) by (node)
    / on(node)
  kube_node_status_capacity{resource="memory"}
)

####Belowed example didn't work on prometheus and OP
Per-node CPU usage percentage:
- 100 * (
  sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (node)
    / on(node)
  kube_node_status_capacity{resource="cpu"}
)
