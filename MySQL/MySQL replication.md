Речь пойдет о репликации MySQL, начнем

---
## 🚀 Get started
Запустим mysql и percona а локальном хосте через `docker compose`
* `docker-compose.yml`
```yaml
version: '3.8'

services:
  percona-slave:
    platform: linux/amd64
    image: percona:8.0
    container_name: percona-slave
    hostname: percona-slave
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: testdb
      MYSQL_USER: replica_user
      MYSQL_PASSWORD: replicapassword
    ports:
      - "3306:3306"
    volumes:
      - ./percona-slave/data:/var/lib/mysql
      - ./percona-slave/config/my.cnf:/etc/mysql/conf.d/my.cnf
    depends_on:
      - mysql-master
    networks:
      - replication-network

  mysql-master:
    platform: linux/amd64
    image: mysql:8.0
    container_name: mysql-master
    hostname: mysql-master
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: testdb
      MYSQL_USER: replica_user
      MYSQL_PASSWORD: replicapassword
    ports:
      - "3307:3306"
    volumes:
      - ./mysql-master/data:/var/lib/mysql
      - ./mysql-master/config/my.cnf:/etc/mysql/conf.d/my.cnf
    networks:
      - replication-network

networks:
  replication-network:
    driver: bridge
```
* `mysql-master/config/my.cnf`
```cnf
[mysqld]
default-time-zone="+03:00"
innodb_file_per_table=1
innodb_flush_log_at_trx_commit=2
sync_binlog=0
# Try to replace O_DIRECT by O_DSYNC if you have "Operating system error number 22"
innodb_flush_method=O_DIRECT
transaction-isolation=READ-COMMITTED
binlog_cache_size=0
sql_mode=""

# ПАРАМЕТРЫ ОТВЕЧАЮЩИЕ ЗА РЕПЛИКАЦИЮ
server-id=2 # ДОЛЖНЫ БЫТЬ УНИКАЛЬНЫМИ
log-bin=mysql-bin
binlog-format=ROW
binlog_do_db=testdb # ИМЯ БД КОТОРУЮ НАДО РЕПЛЕЦИРОВАТЬ 
```

* `percona-slave/config/my.cnf`
```cnf
[mysqld]
server-id = 1 # ДОЛЖНЫ БЫТЬ УНИКАЛЬНЫМИ
replicate-do-db=testdb # ИМЯ БД КОТОРУЮ НАДО РЕПЛЕЦИРОВАТЬ 
```
---
## 🔧Replication settings:
Создали базовую инфру? Отлично, начинаем незабываемый процесс настройки репликации
1. Логинимся в *master* 
```bash
docker exec -ti mysql-master mysql -uroot -prootpassword
```
2. На *master* создаем пользователя для репликации и даем права:
```sql
CREATE USER 'replica_user'@'%' IDENTIFIED WITH mysql_native_password BY 'replicapassword';
GRANT REPLICATION SLAVE ON *.* TO 'replica_user'@'%';
FLUSH PRIVILEGES;
```
3. На *master* если бд в работе блокируем таблицы
```sql
FLUSH TABLES WITH READ LOCK;
```
4. На *master* cнимаем dump
```bash
docker exec -ti sh -c "mysqldump -h 127.0.0.1 -P 3306 -uroot -prootpassword testdb" > testdb.sql
```
5. В *master* смотрим статус репликации!!!
```sql
SHOW MASTER STATUS;
```
Пример вывода, **ЗАПИСЫВАЕМ ВАЖНЫЕ ПАРАМЕТРЫ** ЭТО `File` и `Position`:
```info
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000002 |   699328 | testdb       |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
```
6. На *master* разблокируем таблицы **после снятия дампа**
```sql
UNLOCK TABLES;
```
6. На *slave* накатываем dump:
```bash
docker exec -i percona-slave mysql -uroot -prootpassword testdb < testdb.sql
```
7. Логинимся в *slave*
```bash
docker exec -ti percona-slave mysql -uroot -prootpassword
```
8. На *slave* настраиваем и запускаем репликацию
```sql
CHANGE MASTER TO
  MASTER_HOST='mysql-master',
  MASTER_USER='replica_user',
  MASTER_PASSWORD='replicapassword',
  MASTER_LOG_FILE='<File>', # File ИЗ ПУНКТА 5
  MASTER_LOG_POS=<Position>; # Position ИЗ ПУНКТА 5
START SLAVE;
```
9. На *slave* проверяем статус репликации. Убедитесь, что поля `Slave_IO_Running` и `Slave_SQL_Running` имеют значение `Yes`.
```sql
SHOW SLAVE STATUS\G
```
10. На *master* создаем таблицы
```sql
USE testdb;
CREATE TABLE test_table (id INT PRIMARY KEY, name VARCHAR(50));
INSERT INTO test_table VALUES (1, 'Test Data');
```
11. На *slave* проверяем репликацию
```sql
USE testdb;
SELECT * FROM test_table;
```
12. PROFIT!
