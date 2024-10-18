# Install

## Install using helm

```shell
helm repo add cilium https://helm.cilium.io/
helm install cilium cilium/cilium --version 1.16.3 \
  --namespace kube-system
```

## Install using helmfile

```yaml title=helmfile.yaml
---
environments:
  default:
    kubeContext: defualt
---
repositories:
  - name: cilium
    url: https://helm.cilium.io
releases:
  - chart: cilium/cilium
    namespace: kube-system
    name: cilium
    version: 1.16.3
    values:
      - operator:
        replicas: 1
```

```shell
helmfile sync
```

Restart pods:

```shell
kubectl get pods --all-namespaces -o custom-columns=NAMESPACE:.metadata.namespace,NAME:.metadata.name,HOSTNETWORK:.spec.hostNetwork --no-headers=true | grep '<none>' | awk '{print "-n "$1" "$2}' | xargs -L 1 -r kubectl delete pod
```

Validate install:

```shell
kubectl -n kube-system get pods --watch
```

---

# 🌎 Links

- [Cilium install via helm](https://docs.cilium.io/en/stable/installation/k8s-install-helm/)
- [Cilium cli](https://github.com/cilium/cilium-cli)
- [Tetragon](https://tetragon.io/docs/installation/kubernetes/)

---
