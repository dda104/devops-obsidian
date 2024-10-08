# 📖 Introduction

Sudo позволяет использовать права суперпользователя в текущей сессии

Например:

```shell
sudo dnf install vim -y
```

---

# 😎 Use sudo without entry password

>[!NOTE]
> Ansible требует sudo без пароля для используемого пользователя на целевом хосте

Создать конфигурационный файл

```text title=/etc/sudoers.d/*User_name*
*User_name* ALL=(ALL) NOPASSWD:ALL
```

> [!NOTE] 
> `User_name` - Это имя пользователя для которого необходимо настроить доступ sudo без пароля, например: `user`

---
