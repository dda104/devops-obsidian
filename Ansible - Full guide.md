# 📖 Introduction

Ansible - Это система управления кофигурациями.

# 🤔 Structure of Ansible

## 📚 Example Structure Ansible

Ниже приведен общий пример структуры Ansible, она может выглядить иначе.

```shell
ansible_structure
├── ansible.cfg          - Конфигурационный файл Ansible
├── .ansible-lint        - Настройки Ansible линтера
├── ansible_playbook.yml - Плейбук
├── files/               - Директория со статичными файлами
├── group_vars/          - Директория с переменными для групп узлов
├── host_vars/           - Директория с переменными для отдельных узлов
├── inventory.ini        - Список узлов в формате .ini
├── inventory.yml        - Список узлов в формате .yml
├── LICENSE              - Лицензия
├── README.md            - Документация
├── roles/               - Директория с ролями Ansible
├── templates/           - Директория с шаблонами
└── .yamllint            - Настройки yaml линтера
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

## 📄 Inventory

Ansible Inventory - Это файл со списком хостов в формате ini или yaml, их может быть несколько, могут находиться в произвольной директории и иметь произвольное название.

Пример простого `inventory.yml`:

>[!TIP] Хорошей практикой является использование yaml Inventory

```yaml
---
all:
  hosts:
    server1:
      ansible_host: 10.10.10.10
    server2:
      ansible_host: 20.20.20.20

  children:
    web:
      hosts:
        server1:
        server2:
```

Пример простого `inventory.ini`:

```ini
[web]
server1 ansible_host=10.10.10.10
server1 ansible_host=20.20.20.20
```

## 🗃️ Files

`files/` - Это директория со статичными файлами, может иметь произвольную структуру

Простой пример структуры `files/`

```shell
files/
├── nginx.conf
└── nginx.service
```

## Group and Host vars

В Ansible существуют директории host_vars и group_vars нужны они для большей гибкости использования Ansible, для установки произвольных переменных и их значений для групп узлов и отдельных

>[!INFO] `host_vars/` являются более приоритетными чем 

## Templates

## Roles

---

# Ansible Tasks

---

# Structure Ansible Role

---

# Structure Ansible Collection

---

# Best practice

## Ansible.cfg

## Ansible-lint

## Yamllint

## Ansible doctor

## Ansible galaxy

---

# 🌍 Links

- [Ansible Website](https://www.ansible.com)

---
