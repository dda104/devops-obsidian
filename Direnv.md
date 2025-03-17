# 📖 Introduction

Direnv позваляет создавать автоматически переменные окружения и активировать virtualenv при переходе в требуемуе директорию

Для начала необходимо установить `direnv`, например через  [[ASDF]], также требуется добавить активацию direnv в `<.shellrc>` файл:

```shell title=~/.bashrc 
eval "$(direnv hook bash)"
```

Далее необходимо создать файл `.envrc`:

```shell title=.envrc
#!/not/executable/bash

export VIRTUAL_ENV=".venv"
layout python
```

# 🌎 Links

- [Github direnv](https://github.com/direnv/direnv)
