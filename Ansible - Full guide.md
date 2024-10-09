# ❗ TODO

- [ ] Добавить requirements.yml и requirements.txt
- [ ] Добавить Gathering facts
- [ ] Добавить Prepared variables
- [ ] Написать таски
- [ ] Написать j2
- [ ] Написать структуру коллекции
- [ ] Написать ansible.cfg
- [ ] Написать Ansible lint
- [ ] Написать yamllint
- [ ] Написать ansible doctor
- [ ] Написать Ansible galaxy
- [ ] Написать Molecule
- [ ] Добавить ссылки
- [ ] Добавить pur
- [ ] Добавить Asdf
- [ ] Добавить Эталонный dummy Ansible сюда или в git
- [ ] Добавить Ansible vault
- [ ] Добавить готовые куски ansible сюда или в git
- [ ] Расширить примеры структур
- [ ] Добавить больше инеформации помимо базовых примеров

---

# 📖 Introduction

Ansible - Это система управления кофигурациями.

---

# 📚 Structure of Ansible

Общий пример структуры Ansible:

```shell title=ansible_structure
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

## 📑 Ansible Playbook

Основной исполняемый файл в Ansible - Это ansible playbook.

Структурно плейбук - Это набор плеев, пример Ansible Play:

```yaml title=playbook.yml
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

>[!TIP]
> Хорошей практикой является использование разделителя `_` и расширение `.yml`

## 📄 Inventory

Ansible Inventory - Это файл со списком хостов в формате ini или yaml, их может быть несколько, могут находиться в произвольной директории и иметь произвольное название.

```yaml title=inventory.yml
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

```ini title=inventory.ini
[web]
server1 ansible_host=10.10.10.10
server1 ansible_host=20.20.20.20
```

>[!TIP]
> Хорошей практикой является использование yaml Inventory

## 🗃️ Files

`files/` - Это директория со статичными файлами, может иметь произвольную структуру

```shell title=files/
├── nginx.conf
└── nginx.service
```

## 👥 Group and Host vars

В Ansible существуют директории host_vars и group_vars нужны они для большей гибкости использования Ansible, для установки произвольных переменных и их значений для групп узлов и отдельных

```shell title=group_vars/
├── all.yml
└── web.yml
```

>[!NOTE]
> Переменные в более частных группах являются более приоритетными чем в общих

Простой пример структуры `host_vars/`:

```shell title=host_vars/
├── server1.yml
└── server2.yml
```

>[!NOTE]
> Переменные в `host_vars/` являются более приоритетными чем `group_vars/`

Внутри все файлы переменных между собой похожи, вот простой пример одного из них:

```yaml title=all.yml
---
base_install_pkgs_list:
  - vim
  - git

nginx_install: true
```

## 🧩 Templates

Дикетория `templates/` в структуре Ansible похожа на `files/`, разве, что `templates/` предназначена для шаблонизированных файлов `.j2`.

Пример простой структуры `templates/`:

```shell title=templates/
├── nginx.conf.j2
└── nginx.service.j2
```

Jinja2 (j2) - Это шаблонизатор Python используемый в Ansible.

```j2 title=template.j2
nginx install = {{ nginx_install }}

{% for i in base_install_pkgs_list %}
install package {{ i }}
{% endfor %}
```

# 😎 Advanced

## 📚 Structure Ansible Role

Ansible Role - Структура каталогов содержащая подготовленные таски для переиспользования

Общий пример структуры Ansible роли:

```shell title=ansible_role/
├── defaults/  - Переменные по умолчанию
├── files/     - Статичные файлы
├── handlers/  - Хэндлеры - Таски выполняемые в конце роли при вызове
├── LICENSE    - Лицензия
├── molecule/  - Тесты
├── README.md  - Документация
├── tasks/     - Таски
└── templates/ - Шаблоны
```

## 📄 Ansible Tasks

## 🧩 Jinja2

## 📚 Structure Ansible Collection

---

# Best practice

## 🔧 Ansible.cfg

## 👨‍🏫 Ansible-lint

## 👩‍🏫 Yamllint

## Ansible doctor

## Ansible galaxy

## Molecule

---

# 🌍 Links

- [Ansible Website](https://www.ansible.com)

---
