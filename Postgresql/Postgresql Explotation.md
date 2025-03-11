# db size

```sql
select pg_database_size('databaseName');
```

размер таблиц во всех схемах:

```sql
SELECT schemaname,
       relname AS TABLE_NAME,
       pg_size_pretty(pg_total_relation_size(relid)) AS total_size,
       pg_size_pretty(pg_relation_size(relid)) AS data_size,
       pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid)) AS index_size
FROM pg_catalog.pg_statio_user_tables
ORDER BY pg_total_relation_size(relid) DESC;
```

размер таблицы:

```sql
SELECT schemaname,
       relname AS TABLE_NAME,
       pg_size_pretty(pg_total_relation_size(relid)) AS total_size,
       pg_size_pretty(pg_relation_size(relid)) AS data_size,
       pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid)) AS index_size
FROM pg_catalog.pg_statio_user_tables
ORDER BY pg_total_relation_size(relid) DESC;
```

# Replication

Проверить статус репликации

```sql
select * from pg_stat_replication;
```

Проверить время отставания реплики(выполнять с реплики)

```sql
SELECT now() - pg_last_xact_replay_timestamp();
```

# 🌎 Links

- [отставание реплики](https://snakeproject.ru/rubric/article.php?art=postgresql_delay_10.12.2018)
