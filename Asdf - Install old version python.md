# 📖 Introduction

**Asdf** - Это инструмент на linux для установки в параллель нескольких версий одного языка программирования

---

# ⬇️ Install

Посмотреть последнюю версию во вкладке tags на [Github](https://github.com/asdf-vm/asdf.git)

```shell
git clone https://github.com/asdf-vm/asdf.git <Path to clone> --branch <Git tag>
```

```shell title=~/.bashrc
source "<Path git repository>/asdf.sh"
source "<Path git repository>/completions/asdf.bash"
```

---

# 🐍 Install old version python

1. Установить python plugin в asdf:

```shell
asdf plugin-add python
asdf install python <Semantic Version>
```

2. Установить требуемую версию python:

```shell

```

> [!NOTE]
>  `Version` - Это версия python например `3.11.0`

3. Переключиться на установленную версию:

```shell
asdf global python *Version*
```

---

# 🌎 Links

- [Asdf Website](https://asdf-vm.com)
- [Asdf Github](https://github.com/asdf-vm/asdf.git)

---
