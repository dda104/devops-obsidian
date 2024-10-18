# 🚀 Get started

```shell
curl -s https://fluxcd.io/install.sh | sudo bash
```

Опционально создать репозиторий в gitlab

Создать токен в gitlab

выполнить:

```shell
export GITLAB_TOKEN=<TOKEN>

flux bootstrap gitlab --owner=<group> --repository=<repository name> --path=<path> --read-write-key --components-extra='image-reflector-controller,image-automation-controller'
```

Удалить токен в gitlab

## 🌐 Install Capacitor

создать в папке cluster `capactior.yaml`:

```yaml title=capacitor.yaml
---
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: OCIRepository
metadata:
  name: capacitor
  namespace: flux-system
spec:
  interval: 12h
  url: oci://ghcr.io/gimlet-io/capacitor-manifests
  ref:
    semver: ">=0.1.0"
---
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: capacitor
  namespace: flux-system
spec:
  targetNamespace: flux-system
  interval: 1h
  retryInterval: 2m
  timeout: 5m
  wait: true
  prune: true
  path: "./"
  sourceRef:
    kind: OCIRepository
    name: capacitor
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: capacitor-ingress
  namespace: flux-system
spec:
  policyTypes:
    - Ingress
  ingress:
    - from:
      - namespaceSelector: {}
  podSelector:
    matchLabels:
      app.kubernetes.io/instance: capacitor
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: capacitor
  namespace: flux-system
spec:
  ingressClassName: "nginx"
  rules:
    - host: capacitor.google.com
      http:
        paths:
          - pathType: Prefix
            path: /
            backend:
              service:
                name: capacitor
                port:
                  number: 9000
```

## 🧩 Install Helm Chart

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
kind: Namespace
metadata:
  name: monitoring
  labels:
    name: monitoring
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
    values:
    vmsingle:
      ingress:
        enabled: true
        ingressClassName: "nginx"
        hosts:
          - vm.google.com
```

Для проверки можно использовать:

```shell
flux get all
```

## Uninstall

```shell
flux uninstall
```

# 🌎 Links


- [FluxCD в действии - Георг Гаал](https://www.youtube.com/watch?v=T4fkWIGahiQ&t=711s)

---
