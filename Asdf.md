# 📖 Introduction

**asdf** - Это инструмент на linux для установки в параллель нескольких версий одного языка программирования

---

# ⬇️ Install

1. Посмотреть последнюю версию во вкладке tags https://github.com/asdf-vm/asdf.git
2. Скачать репозиторий

```shell
git clone https://github.com/asdf-vm/asdf.git *path* --branch *version*
```

>[!INFO] `path` - Это путь куда склонируется репозиторий, в документации рекомендуется `~/.asdf 

>[!INFO] `version` - Это версия из 1 пункта

3. Добавить автозапуск на примере bash

---

# 🐍 Install old version python

1. Установить python plugin в asdf:

```shell
asdf plugin-add python
```

2. Установить требуемую версию python:

```shell
asdf install python *version*
```

>

3. Переключиться на установленную версию: `asdf global python 3.11.0`