# 📖 Introduction

- **asdf** - Это инструмент на linux для установки в параллель нескольких версий одного языка программирования

---

# ⬇️ Install

Посмотреть последнюю версию во вкладке tags на [Github asdf](https://github.com/asdf-vm/asdf.git)

```shell
git clone https://github.com/asdf-vm/asdf.git <Path_to_clone> --branch <Git_tag>
```

```shell title=~/.bashrc
source <Path_git_repository>/asdf.sh
source <Path_git_repository>/completions/asdf.bash
```

---

# 👨‍🏭 Usage

Пример использования: глобальная установка python

```shell title=Python
asdf plugin-add python
asdf install python <Semantic_version>
asdf global python <Semantic_version>
```

# ✍️ Use case

Предлагается в гит репозиториях заводить файл `.tool-versions` с указанием используемых инструментов и их версий, например:

```shell title=.tool-versions
python 3.13.2
direnv 2.35.0
task 3.41.0
goss 0.4.9
```

---

# 🌎 Links

- [asdf Website](https://asdf-vm.com)
- [asdf Github](https://github.com/asdf-vm/asdf.git)

---
