---
sticker: emoji//1f427
color: var(--mk-color-orange)
---
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
openssl enc -aes-256-cbc -salt -pbkdf2 -in <File_path> -out <Encrypted_file> -k <Password>
```

Расшифровать файл:

```shell
openssl enc -d -aes-256-cbc -pbkdf2 -in <Encrypted_file> -k <Password>
```

---

# 🌎 Links

---
