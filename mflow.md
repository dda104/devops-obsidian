# 🚀 Get started

Самый обычный запуск mlflow на локальном хосте:

```shell
mlflow server --host 0.0.0.0 --port *5000/8080*
```
Проверка что работает
1. Создаем venv
```shell
virtualenv .venv
source .venv/bin/activate
```
2. Устанавливаем зависимости
```shell
pip install numpy scikit-learn==1.5.1 mlflow
```
3. Экспортируем переменные окружения
```shell
   export MLFLOW_TRACKING_URI=http://127.0.0.1:8080
```
4. Создаем файл для эксперимента `test.py`
  ```python
import mlflow

# import logging

# logger = logging.getLogger("mlflow")

# logger.setLevel(logging.DEBUG)

  

from sklearn.model_selection import train_test_split

from sklearn.datasets import load_diabetes

from sklearn.ensemble import RandomForestRegressor

  

mlflow.autolog()

  

db = load_diabetes()

X_train, X_test, y_train, y_test = train_test_split(db.data, db.target)

  

# Create and train models.

rf = RandomForestRegressor(n_estimators=100, max_depth=6, max_features=3)

rf.fit(X_train, y_train)

  

# Use the model to make predictions on the test dataset.

predictions = rf.predict(X_test)
```
5.  Запускаем и смотрим в вебке, PROFIT!
---

# 🔧 Production example

Вам стало скучно? В жизни не хватает секса? Тогда этот раздел для вас!
docker compose для запуска mlflow с S3 и postgresql
```yaml
services:
  postgres:
    container_name: postgres
    hostname: postgres
    image: 'postgres:16.4'
    restart: always
    environment:
        POSTGRES_USER: postgres
        POSTGRES_PASSWORD: postgres
    ports:
        - '127.0.0.1:5432:5432'
    volumes:
      - '/srv/postgres/data:/var/lib/postgresql/data'
    networks:
      - apps
services:
  minio:
    container_name: minio
    hostname: minio
    image: minio/minio:RELEASE.2024-10-02T17-50-41Z-cpuv1
    restart: always
    environment:
      - MINIO_ROOT_USER=username
      - MINIO_ROOT_PASSWORD=password
    ports:
        - '9000:9000'
        - '9001:9001'
    volumes:
      - /srv/minio:/data
    command: server /data --console-address :9001 --address :9000
    networks:
      - apps
mlflow:
    container_name: mlflow
    hostname: mlflow
    image: ghcr.io/mlflow/mlflow:v2.16.2
    restart: always
    environment:
      MLFLOW_S3_ENDPOINT_URL: http://minio:9000
      AWS_ACCESS_KEY_ID: username
      AWS_SECRET_ACCESS_KEY: password
      MLFLOW_AUTH_CONFIG_PATH: /config/basic_auth.ini
    ports:
        - '5000:5000'
    volumes:
      - /srv/mlflow/config:/config:rw
    command: >
      /bin/bash -c "
      pip install psycopg2-binary boto3 &&
      mlflow server --host 0.0.0.0 --backend-store-uri postgresql://postgres:postgres@postgres/*POSTGRES MLFLOW DATABASE* --artifacts-destination s3://*S3 BUSKET*/mlflow --app-name basic-auth --gunicorn-opts  '--log-level debug'"
    networks:
      - apps
networks:
  apps:
    name: apps
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: "172.100.0.0/24"
          gateway: "172.100.0.1"
```
basic_auth.ini
```ini
[mlflow]
default_permission = READ
database_uri = postgresql://postgres:postgre@postgres/*POSTGRES MLFLOW AUTH DATABASE*
admin_username = admin
admin_password = password
authorization_function = mlflow.server.auth:authenticate_request_basic_auth
```

---

# 👇 Possible problems

* На macos 5000 port  по умолчанию занят системным сервисов, лучше использовать 8080
* Если вначале был запущен backend без S3, после этого придется чистить в базе в таблице `public.runs` колонку `artifact_uri` тк она будет содержать `/mlartifacts/` (артефакты не будут помещаться в S3)
---

#  🌎 Links

* https://mlflow.org/docs/latest/tracking/tutorials/remote-server.html
* https://github.com/mlflow/mlflow/tree/master/examples/mlflow_artifacts
* https://github.com/mlflow/mlflow/tree/master/examples/docker
* https://github.com/sachua/mlflow-docker-compose/tree/master
* https://mlflow.org/docs/latest/tracking.html
* https://github.com/mlflow/mlflow/issues/9695
* https://mlflow.org/docs/latest/tracking/tutorials/remote-server.html
* https://www.restack.io/docs/mlflow-knowledge-mlflow-log-model-s3
* https://mlflow.org/docs/latest/auth/index.html

---
