- uv - Инструмент заменяющий группу инструментов для разработки на python, ansible, pulumi и т.п.

> - 🚀 A single tool to replace `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv`, and more.

Установим uv через [[ASDF]]:

```shell title=.tool-versions
python 3.13.2
uv 0.6.6
```

Подготовим проект для ansible в uv:

```shell
uv init
uv add ansible
uv add ansible-core
uv add molecule
uv add molecule-plugins[podman]
uv add ansible-doctor
uv add ansible-lint
uv add yamllint
```


# 🌎 Links

- [Github uv](https://github.com/astral-sh/uv)
