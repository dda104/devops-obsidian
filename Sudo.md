# 📖 Introduction

Sudo позволяет использовать права суперпользователя в текущей сессии:

```shell title=Usage
sudo <command>
```

```shell title=Example
sudo dnf up -y
```

---

# 😎 Use sudo without entry password

>[!NOTE]
> Ansible требует sudo без пароля для используемого пользователя на целевом хосте

Создать конфигурационный файл

```text title=/etc/sudoers.d/<Username>
<Username> ALL=(ALL) NOPASSWD:ALL
```

---
