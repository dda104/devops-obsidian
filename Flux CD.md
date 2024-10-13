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

создать в папке cluster `<resource>.yaml`, например `vm.yaml`

Для проверки можно использовать:

```shell
flux get all
```
