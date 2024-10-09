# Selinux

##

Для управления selinux можно использовать semanage

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

# 🌎 Links

---
