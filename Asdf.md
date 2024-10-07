# 📖 Introduction

**Asdf** - Это инструмент на linux для установки в параллель нескольких версий одного языка программирования

---

# ⬇️ Install

1. Посмотреть последнюю версию во вкладке tags на [Github](https://github.com/asdf-vm/asdf.git)
2. Скачать репозиторий

```shell
git clone https://github.com/asdf-vm/asdf.git *Path* --branch *Version*
```

>[!INFO] `Path` - Это путь куда склонируется репозиторий, в документации рекомендуется `~/.asdf 

>[!INFO] `Version` - Это версия из 1 пункта

3. Добавить автозапуск на примере bash

```shell
{
    echo 'source "*Path*/asdf.sh"'
    echo 'source "*Path*/completions/asdf.bash"'
} >> ~/.bashrc
```

>[!INFO] `Path` - Это путь до репозитория в файловой системе

---

# 🐍 Install old version python

1. Установить python plugin в asdf:

```shell
asdf plugin-add python
```

2. Установить требуемую версию python:

```shell
asdf install python *Version*
```

> [!INFO] `Version` - Это версия python например `3.11.0`

3. Переключиться на установленную версию:

```shell
asdf global python *Version*
```

---

# 🌎 Links

[Asdf website](https://asdf-vm.com)
[Asdf github](https://github.com/asdf-vm/asdf.git)

---
