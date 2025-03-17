Direnv позваляет создавать автоматически переменные окружения и активировать virtualenv при переходе в требуемуе директорию

Для начала необходимо установить `direnv`, например через [[ASDF]]

Далее необходимо создать файл `.env` или `.envrc`:

```shell title=.envrc
#!/not/executable/bash

export VIRTUAL_ENV=".venv"
layout python3 .venv
```
