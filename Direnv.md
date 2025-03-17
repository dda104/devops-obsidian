


```shell 
eval "$(direnv hook bash)"
```

Далее необходимо создать файл `.envrc`:

```shell title=.envrc
#!/not/executable/bash

export VIRTUAL_ENV=".venv"
layout python
```
