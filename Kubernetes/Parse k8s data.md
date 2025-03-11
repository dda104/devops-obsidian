# disks

```shell
kubectl get --raw "/api/v1/nodes/dtw-dc05-kube010s.passport.local/proxy/stats/summary" > test
```

```shell
cat test | jq -r '.pods[] | select (.podRef.name) | { name: .podRef.name, rootfsUsedBytes: .containers[].rootfs.usedBytes, logsUsedBytes: .containers[].logs.usedBytes, volumeUsedBytes: .volume[].usedBytes, ephemeralStorageUsedBytes: .ephemeralstorage.usedBytes }' | jq -n 'reduce inputs as $row ([]; . + [$row])'
```

```shell
cat test | jq -r '.pods[] | "PodName: ", .podRef.name, "usedBytes: ", .containers[].rootfs.usedBytes' | jq -nc 'reduce inputs as $row ([]; . + [$row])'
```
# requests per pod

```shell
kubectl get pod --all-namespaces --sort-by='.metadata.name' -o json | jq -r '[.items[] | {pod_name: .metadata.name, namespace: .metadata.namespace, containers: .spec.containers[] | [ {container_name: .name, memory_requested: .resources.requests.memory, cpu_requested: .resources.requests.cpu,memory_limits: .resources.limits.memory, cpu_limits: .resources.limits.cpu} ] }]' | jq  'sort_by(.containers[0].cpu_requested)'
```
