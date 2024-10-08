# 📖 Introduction

Ansible - Это система управления кофигурациями.

# 🤔 Structure of Ansible

## 📚 Example Structure Ansible

Ниже приведен общий пример структуры Ansible, она может выглядить иначе.

```shell

```

Далее элементы структуры будут описаны подробнее.

## 📑 Ansible Playbook

Основной исполняемый файл в Ansible - Это ansible playbook.

Структурно плейбук - Это набор плеев, пример Ansible Play:

```yaml
---
- name: Simple play
  hosts: all
  tasks:
    - name: Hello World
      ansible.builtin.debug:
        msg: Hello World
```

Ansible Play - Это структура содержащая набор тасков и/или ролей выполняемые на определенной группе узлов.

Ansible Playbook может иметь произвольное название с расширением `.yml` или `.yaml`

>[!TIP] Хорошей практикой является использование разделителя `_` и расширение `.yml`

---

# 🌍 Links

- [Ansible Website](https://www.ansible.com)

---
