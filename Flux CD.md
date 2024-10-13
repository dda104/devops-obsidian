```shell
curl -s https://fluxcd.io/install.sh | sudo bash
```

Создать репозиторий в gitlab

Создать токен в gitlab

выполнить:
`
```shell
export GITLAB_TOKEN=<TOKEN>

flux bootstrap gitlab --owner=<group> --repository=<repository name> --path=<path> --read-write-key --components-extra='image-reflector-controller,image-automation-controller'
```

Удалить токен в gitlab

создать в папке cluster `<resource>.yaml`:

```yaml title=hr-vm.yaml
---
apiVersion: source.toolkit.fluxcd.io/v1
kind: HelmRepository
metadata:
  name: vm
  namespace: monitoring
spec:
  interval: 1m0s
  url: https://victoriametrics.github.io/helm-charts/
---
apiVersion: v1
king: Namespace
metadata: monitoring
---
apiVersion: helm.toolkit.fluxcd.io/v2
kind: HelmRelease
metadata:
  name: victoria-metrics-k8s-stack
  namespace: monitoring
spec:
  chart:
    spec:
      chart: victoria-metrics-k8s-stack
      sourceRef:
        kind: HelmRepository
        name: vm
      version: '*'
  interval: 1m0s
```

Для проверки можно использовать:

```shell
flux get all
```
