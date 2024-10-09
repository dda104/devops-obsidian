# Selinux

## Semanage

Для управления selinux можно использовать semanage, для начала его нужно установить:

```shell title=fedora
dnf in semanage -y
```

Разрешить использование порта для ssh:

```shell
semanage port -a -t ssh_port_t -p tcp 666
```

---

# Firewalld

Разрешить порт:

```shell
firewall-cmd --permanent --add-port=666/tcp
firewall-cmd --reload
```

---

# Encrypting via Openssh

Зашифровать файл:

```shell
openssl enc -aes-256-cbc -salt -pbkdf2 -in <File_path> -out <Output_file.enc> -k <Password>
```

---

# 🌎 Links

---
