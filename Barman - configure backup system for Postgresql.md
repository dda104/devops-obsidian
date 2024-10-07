# Преднастрой на Postgresql

Требуется создать У.З. например `barman`

```sql
CREATE user barman PASSWORD 'barman';
ALTER USER barman WITH SUPERUSER;
```

Внести изменения в `/etc/postgresql/*version*/main/postgresql.conf`

```ini
wal_level = replica or logical
max_wal_senders > 3
max_replication_Slots > 3
listen_addresses='*'
```

> [!INFO] max_wal_senders и max_replication_slots изначально 10, wal_level по умолчанию replica

Добавить пользователя в `/etc/postgres/*version*/main/pg_hba.conf`

---

# Установка Barman на примере Debian 11

## Установка пакета

```shell
apt update
apt -y install curl ca-certificates gnupg
curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
apt update
apt -y install barman
```

## Изменение конфигурационных файлов

Отредактировать `/etc/barmsn/barman.conf`

```ini
barman_home = /mnt/backups
immediate_checkpoint = true
retention_policy = RECOVERY WINDOW OF 1 WEEK
log_level = INFO
compression = gzip
basebackup_retry_times = 2
basebackup_retry_sleep = 30
```

Создать `/etc/barman.d/postgres.conf`

```ini
[pg_server]
description = "Streaming Backups using pg_basebackup and pg_recievewal for archiving wal files"
conninfo = host=10.0.1.1 user=barman dbname=postgres port=5432
streaming_conninfo = host=10.0.1.1 user=barman dbname=postgres port=5432
backup_method = postgres
streaming_archiver = on
slot_name = barman_slot
create_slot = auto
```

Создать ~barman/.pgpass

```text
10.0.1.1:5432:*:barman:barman
```

Настроить и проверить бэкапирование

```shell
barman cron
barman recieve-wal --create-slot postgres
barman switch-xlog --force --archive postgres
barman check postgres
```

> [!INFO] Должно быть все OK кроме количества бэкапов

---
# Эксплуатационные задачи

## Просмотр списка бэкапов

```shell
barman list-backup all
```

## Ручной бэкап

```shell
barman backup postgres
```

## Удалить бэкап

Получить id `barman list-backup all`

```shell
barman list-backup all
barman delete *id*
```

## Восстановление из бэкапа

```shell
barman list-backup all
barman recover --remote-ssh-command "ssh postgres@*remote_ip*" postgres_server *id_barman_backup* /*remote_path*/*version*/main
```

> [!INFO] Если использовать другого пользователя придется выполнить
> `chown postgres:postgres -R /*remote_path*/*version*/main`

## Смена сервера с которого снимается backup

1. Изменить файлы `/etc/barman.d/postgres.conf` и `~barman/.pgpass`
2. Пересоздать слот

```shell
barman recieve-wal --delete-slot postgres
barman cron
barman recieve-wal --create-slot postgres
barman check postgres
```


---

# Links

[Configure guide](https://stormatics.tech/alis-planet-postgresql/postgresql-backup-and-recovery-management-using-barman)

---
