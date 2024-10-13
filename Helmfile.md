# 🚀 Get started

- **Helmfile** - Это утилита которая позволяет использовать IaC подход в развертывании helm чартов

## ⬇️ Install

Перейти в Releases на [Github Helmfile](https://github.com/helmfile/helmfile) и скачать требуемую версию для требуемой системы и перенести исполняемый файл в директорию из $PATH, например: `/usr/bin`

## 👨‍🏭 Usage

Для начала создать `helmfile.yaml`:

```yaml
---
environments:
  default:
    kubeContext: my_kubernetes
---
repositories:
  - name: ingress-nginx
    url: https://kubernetes.github.io/ingress-nginx
releases:
  - chart: ingress-nginx/ingress-nginx
    namespace: ingress-nginx
    name: ingress-nginx
    version: ~4.11.0
    values:
      - controller:
          service:
            loadBalancerIP: 10.10.10.10

```

> [!NOTE]
> Рекомендуется разделять environments и releases:
> WARNING: environments and releases cannot be defined within the same YAML part. Use --- to extract the environments into a dedicated part

