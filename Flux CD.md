```shell
curl -s https://fluxcd.io/install.sh | sudo bash
```

Создать репозиторий в gitlab

выполнить:
`
```shell
flux bootstrap gitlab --owner=<group> --repository=<repository name> --path=<path> --read-write-key --components-extra='image-reflector-controller,image-automation-controller'
```

