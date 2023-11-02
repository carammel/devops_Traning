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
### PromQL difference between memory usage and working set
The difference between memory usage and working set in PromQL is that the memory usage means the total amount of memory allocated to a process, while working set is the amount of memory that is actively being used by the process. Memory usage includes both active and inactive memory, while the working set only includes the memory that is being actively used. In other words, the working set is a subset of the memory usage.
The following expression can be used to sum the average pod container memory usage by namespace and pod over a two day period:

- sum by (namespace, pod) (avg_over_time(pod:container_memory_usage_bytes:sum[2d]))

Recommendation is to use the following query because memory_working_set will give the memory usage which is actively used.
- sum by (namespace, pod) (avg_over_time(node_namespace_pod_container:container_memory_working_set_bytes{}[2h])/1048576)
