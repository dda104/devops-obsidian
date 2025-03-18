- Molecule - инструмент тестирования ролей в ansible

Ожидается, что роль уже написана и имеет вид:

```shell
ansible_role
├── meta
│   └── main.yml
├── README.md
├── ...
└── tasks
    ├── ...
    └── main.yml
```

Установим uv через [[ASDF]]:

```shell title=.tool-versions
python 3.13.2
uv 0.6.6
```

```shell
cat .tool-versions | awk '{print $1}' | xargs -i asdf plugin add {}
asdf install
```

Подготовим проект в uv:

```shell
uv init
uv add ansible
uv add ansible-core
uv add molecule
uv add molecule-plugins[docker]
```

> [!note]
> `uv add molecule-plugins[docker]`, docker можно заменить на другие драйверы: podman, azure, containers, ec2, gce, openstack, vagrant
> [Github molecule-plugins](https://github.com/ansible-community/molecule-plugins)

> [!note]
> Чтобы скачать зависимости после клонирования репозитория выполнить:
> `uv sync`

Обновим `.gitignore`

```diff title=.gitignore
+ .vscode
+ .idea
+ .ansible

# Python-generated files
__pycache__
*.py[oc]
build/
dist/
wheels/
*.egg-info

# Virtual environments
.venv
```

Создадим простой сценарий molecule:

```shell
ansible_role
├── meta
│   └── main.yml
├── molecule
│   └── default
│       ├── converge.yml
│       ├── molecule.yml
│       └── verify.yml
├── README.md
├── ...
└── tasks
    ├── ...
    └── main.yml
```

- `molecule/` - директория для молекулы
- `molecule/default/` - default это название сценария, по умолчанию это default
- `molecule/default/molecule.yml` - основной конфиг для молекулы

- `molecule/default/converge.yml` - плейбук для развертывания
- `molecule/default/verify.yml` - плейбук для тестирования после развертывания

Рассмотрим файлы:

```yaml title=molecule.yml
---
driver:
  name: docker # Название драйвера как в molecule-plugins[<driver>]

platforms:
  - name: ansible_role.fedora41.molecule # Название создаваемого докер контейнера
    image: docker.io/geerlingguy/docker-fedora41-ansible:latest # Специально подготовленный образ для тестирования ansible
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw # Необходимо для поддержки sytemctl/systemd внутри контейнера
    cgroupns_mode: host
    command: /usr/sbin/init
    privileged: true
    pre_build_image: true

  - name: ansible_role.ubuntu2404.molecule
    image: docker.io/geerlingguy/docker-ubuntu2404-ansible:latest
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw
    cgroupns_mode: host
    command: /usr/sbin/init
    privileged: true
    pre_build_image: true

provisioner:
  name: ansible
  config_options: # Это замена ansible.cfg
    defaults:
      result_format: yaml
      callbacks_enabled: profile_tasks
      gathering: "smart"
      fact_caching: jsonfile
      fact_caching_connection: "./.facts"
      fact_caching_timeout: 86400
      forks: 20
  env: # Создаваемые переменные окружения во время тестирования
    ANSIBLE_ROLES_PATH: "../../../" # Необходимо для того чтобы molecule смогла найти роли
    ANSIBLE_PIPELINING: true
  playbooks:
    converge: converge.yml # Плейбук для развертывания
    verify: verify.yml # Плейбук для тестирования

scenario:
  name: default # Название сценария, при создании дополнительного сценария изменить название
  test_sequence:
    - destroy # Можно вызвать destroy перед create чтобы удалить оставшиеся контейнеры например при некорректом завершении работы
    - create # Создание контейнеров
    - converge # Развертывание
    - idempotence # Проверка на идемпотентность
    - verify # Выполнить проверку
    - destroy # Удалить контейнеры

verifier:
  name: ansible # Инструмент с помощью которого будет выполняться проверка, также доступны goss и testinfra
```

Чистый от комментариев конфиг:

```yaml title=molecule.yml
---
driver:
  name: docker

platforms:
  - name: ansible_role.fedora41.molecule
    image: docker.io/geerlingguy/docker-fedora41-ansible:latest
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw
    cgroupns_mode: host
    command: /usr/sbin/init
    privileged: true
    pre_build_image: true

  - name: ansible_role.ubuntu2404.molecule
    image: docker.io/geerlingguy/docker-ubuntu2404-ansible:latest
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw
    cgroupns_mode: host
    command: /usr/sbin/init
    privileged: true
    pre_build_image: true

provisioner:
  name: ansible
  config_options:
    defaults:
      result_format: yaml
      callbacks_enabled: profile_tasks
      gathering: "smart"
      fact_caching: jsonfile
      fact_caching_connection: "./.facts"
      fact_caching_timeout: 86400
      forks: 20
  env:
    ANSIBLE_ROLES_PATH: "../../../"
    ANSIBLE_PIPELINING: true
  playbooks:
    converge: converge.yml
    verify: verify.yml

scenario:
  name: default
  test_sequence:
    - destroy
    - create
    - converge
    - idempotence
    - verify
    - destroy

verifier:
  name: ansible
```

```yaml title=converge.yml
---
- name: Converge
  hosts: all
  roles:
    - ansible_role
```

```yaml title=verify.yml
---
- name: Verify
  hosts: all
  tasks:
    ...
```

> [!note]
>  verify не является обязательным шагом в molecule, можно написать свой плейбук для проверки хоста, например наличие пакетов, состояние systemd сервисов и т.п.

Выполним тестирование:

Чтобы запустить тест необходимо перейти в директорию роли и вызвать `molecule test`

```shell
cd ansible_role/
uv run molecule test
```

> [!note]
>  При использовании uv следует вызывать инструменты с помощью `uv run`

Чтобы после тестирования не удалялись контейнеры:

```shell
uv run molecule test --destroy=never
```

Удалить контейнеры после тестов:

```shell
molecule destroy
```

Чтобы вызвать конкретный шаг:

```shell
molecule verify
```

Чтобы вызвать нестандартный сценарий:

```shell
molecule -s some_scenario_name test
```

# 🌎 Links

- [Github molecule-plugins](https://github.com/ansible-community/molecule-plugins)
