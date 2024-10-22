# 📖 Introduction

**Barman** - Это инструмент для настройки регулярных бэкапов, позиционируется как recovery system.

---

# 💼 Prepare Postgresql

## 🏗️ Create Role

```sql
CREATE user <Role_name> PASSWORD '<Role_password>';
ALTER USER <Role_name> WITH SUPERUSER;
```

Добавить role в `/etc/postgres/<Version postgresql>/main/pg_hba.conf`

## 🔧 Configure Postgresql

Внести изменения в `/etc/postgresql/<Version postgresql>/main/postgresql.conf`

```ini title=postgresql.conf
wal_level = replica or logical
max_wal_senders > 3
max_replication_Slots > 3
listen_addresses='<IP_address_barman_host>'
```

---

# 🔨 Configure Barman on Debian 11

## ⬇️ Install Package

```shell
apt update
apt -y install curl ca-certificates gnupg
curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
apt update
apt -y install barman
```

## 
Configure Barman

Отредактировать `/etc/barman/barman.conf`

```ini title=/etc/barman/barman.conf
barman_home = /mnt/backups
immediate_checkpoint = true
retention_policy = RECOVERY WINDOW OF 1 WEEK
log_level = INFO
compression = gzip
basebackup_retry_times = 2
basebackup_retry_sleep = 30
```

Создать `/etc/barman.d/postgres.conf`

```ini title=/etc/barman.d/postgres.conf
[pg_server]
description = "Streaming Backups using pg_basebackup and pg_recievewal for archiving wal files"
conninfo = host=10.0.1.1 user=barman dbname=postgres port=5432
streaming_conninfo = host=10.0.1.1 user=barman dbname=postgres port=5432
backup_method = postgres
streaming_archiver = on
slot_name = barman_slot
create_slot = auto
```

Создать `~barman/.pgpass`

```text title=~barman/.pgpass
10.0.1.1:5432:*:<Barman User>:<Barman Password>
```

Настроить и проверить бэкапирование

```shell
barman cron
barman recieve-wal --create-slot postgres
barman switch-xlog --force --archive postgres
barman check postgres
```

> [!NOTE]
>  Должно быть все OK кроме количества бэкапов

---
# 👨‍🏭 Usage

## 👀 Get List Existing Backups

```shell
barman list-backup all
```

## 🔨 Manual Backup

```shell
barman backup postgres
```

## ❌ Delete Backup


```shell
barman list-backup all
barman delete <ID_backup>
```

## ♻️ Restore From Backup

```shell
barman list-backup all
barman recover --remote-ssh-command "ssh postgres@<Remote_ip>" postgres_server <ID_barman_backup> /<Remote_path>/<Version_postgresql>/main
```

> [!NOTE]
>  Если использовать другого пользователя придется выполнить
> `chown postgres:postgres -R /Remote_path>/<Version_posgresql>/main`

## 🔧 Change Target Server

Для смены сервера с которого снимается бэкап:

1. Изменить файлы `/etc/barman.d/postgres.conf` и `~barman/.pgpass`
2. Пересоздать слот

```shell
barman recieve-wal --delete-slot postgres
barman cron
barman recieve-wal --create-slot postgres
barman check postgres
```

---

# 🌎 Links

- [Barman Website](https://pgbarman.org)
- [Configure Guide](https://stormatics.tech/alis-planet-postgresql/postgresql-backup-and-recovery-management-using-barman)

---
