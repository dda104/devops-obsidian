---
color: var(--mk-color-teal)
---
# Install K3S

Simple Install

```shell
curl -sfL https://get.k3s.io | sh -
```

Install without traefik

```shell
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --disable traefik" sh
```

Install without flannel

```shell
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC='--flannel-backend=none --disable-network-policy' sh -
```

Install without traefik and flanel

```shell
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC='server --disable traefik --flannel-backend=none --disable-network-policy' sh -
```

---

# 🌎 Links

- [Quick Start K3S](https://docs.k3s.io/quick-start)
- [Install cilium in k3s](https://docs.cilium.io/en/stable/installation/k3s/)

---
