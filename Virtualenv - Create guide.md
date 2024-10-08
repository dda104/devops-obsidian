# 📖 Introduction

- **Virtual environment (venv)** - Это модуль для python нужен для создания виртуальных окружений.

Виртуальные окружения прежде всего удобны тем, что позволяют устанавливать библиотеки python различных версий которые не будут конфликтовать с системными.

---

# 💼 Usage

1️⃣ Создание виртуального окружения:

```shell
python3 -m venv *venv_name*
```

>[!INFO] 
>`venv_name` - Это произвольное название виртуального окружения, часто используется `.venv`

>[!TIP] 
>Директорию `venv_name` следует добавить в `.gitignore`

2️⃣ Активация виртуального окружения:

```shell
source *venv_name*/bin/activate
```

3️⃣ Выход из виртуального окружения:

```shell
deactivate
```

> [!INFO]
> В vscode эта команда недоступна

---

# 🌎 Links

- [PiPy virtualenv](https://pypi.org/project/virtualenv/)

---
