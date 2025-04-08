[quickwit.io](https://quickwit.io) - пока что самый быстрый и вместительный сервис по хранению логов и трейсов.
Binance держит на нем [100Pb данных](https://quickwit.io/blog/quickwit-binance-story)

# Установка в кубер

## Если у нас нет S3 

- Используем - [local-path-provisioner](https://github.com/rancher/local-path-provisioner)
- Создаем выделенный StorageClass, который поможет переиспользовать одну и ту же папку на хосте
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-path-ns-folder
provisioner: rancher.io/local-path
parameters:
  pathPattern: '{{ .PVC.Namespace }}'
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

## Вельюсы чарты
```yaml
image:
  repository: <repo>/quickwit/quickwit

environment:
  QW_METASTORE_URI: file:///quickwit/qwdata/indexes
  QW_DISABLE_TELEMETRY: 1

config:
  default_index_root_uri: file:///quickwit/qwdata/indexes

tolerations:
  - key: node-role.kubernetes.io/infra
    operator: "Equal"
    effect: "NoSchedule"

searcher:
  nodeSelector:
    node-role.kubernetes.io/infra: ""
  persistentVolume:
    enabled: true
    storage: "30Gi"
    storageClass: "local-path-ns-folder"
indexer:
  nodeSelector:
    node-role.kubernetes.io/infra: ""
  persistentVolume:
    enabled: true
    storage: "30Gi"
    storageClass: "local-path-ns-folder"
metastore:
  nodeSelector:
    node-role.kubernetes.io/infra: ""
control_plane:
  nodeSelector:
    node-role.kubernetes.io/infra: ""
janitor:
  nodeSelector:
    node-role.kubernetes.io/infra: ""
bootstrap:
  enabled: true
  nodeSelector:
    node-role.kubernetes.io/infra: ""

ingress:
  enabled: true
  className: nginx
  hosts:
    - host: "<host>"
      paths:
        # Ingest and ES bulk endpoints to quickwit-indexer
        - path: /api/v1/.*/ingest
          pathType: ImplementationSpecific
          service: indexer
          port: rest
        - path: /api/v1/_elastic/bulk
          pathType: ImplementationSpecific
          service: indexer
          port: rest
        - path: /api/v1/_elastic/.*/_bulk
          pathType: ImplementationSpecific
          service: indexer
          port: rest
        # Indexes API endpoints to quickwit-metastore
        - path: /api/v1/indexes
          pathType: Prefix
          service: metastore
          port: rest
        # Everything else to quickwit-searcher
        - path: /
          pathType: ImplementationSpecific
          service: searcher
          port: rest
  tls:
    - hosts:
        - <host>
      secretName: tls-test

# Если нужен индекс для fluentbit
seed:
  indexes:
    - version: "0.8"

      index_id: fluentbit-logs
      retention:
        period: 3 days
        schedule: daily

      doc_mapping:
        mode: dynamic
        field_mappings:
          - name: timestamp
            type: datetime
            input_formats:
              - unix_timestamp
            output_format: unix_timestamp_secs
            fast: true
        timestamp_field: timestamp

      indexing_settings:
        commit_timeout_secs: 10
```