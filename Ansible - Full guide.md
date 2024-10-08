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

## Inventory

## Files

## Group and Host vars

## Templates

## Roles

---

# 

# Best practice

## Ansible galaxy

## Ansible.cfg

## Ansible-lint

## Yamllint

## Ansible doctor

---

# 🌍 Links

- [Ansible Website](https://www.ansible.com)

---
