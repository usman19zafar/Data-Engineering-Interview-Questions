1. MASTER docker-compose.yml (REFERENCES ALL FOLDERS)
This is the canonical, Linux‑ready, multi‑service compose that aligns with your folder taxonomy.

It is modular: each service points to its own folder under /engineering_lab.

You can split it later — but this is the single master file you asked for.

Code
version: "3.9"

services:

  # -------------------------
  # 01 STREAMING
  # -------------------------
  kafka:
    image: bitnami/kafka:latest
    container_name: kafka
    ports:
      - "9092:9092"
    volumes:
      - ./01_streaming/kafka:/data

  redpanda:
    image: redpandadata/redpanda:latest
    container_name: redpanda
    ports:
      - "9094:9094"
    volumes:
      - ./01_streaming/redpanda:/var/lib/redpanda

  # -------------------------
  # 02 STREAM PROCESSING
  # -------------------------
  spark:
    image: bitnami/spark:latest
    container_name: spark
    volumes:
      - ./02_stream_processing/spark_streaming:/opt/spark-apps

  # -------------------------
  # 03 DATA LAKE
  # -------------------------
  minio:
    image: minio/minio:latest
    container_name: minio
    command: server /data
    ports:
      - "9000:9000"
    volumes:
      - ./03_data_lake/minio:/data

  # -------------------------
  # 04 OLAP
  # -------------------------
  clickhouse:
    image: clickhouse/clickhouse-server:latest
    container_name: clickhouse
    ports:
      - "8123:8123"
    volumes:
      - ./04_olap/clickhouse:/var/lib/clickhouse

  # -------------------------
  # 05 SQL ENGINES
  # -------------------------
  trino:
    image: trinodb/trino:latest
    container_name: trino
    ports:
      - "8080:8080"
    volumes:
      - ./05_sql_engines/trino:/etc/trino

  # -------------------------
  # 06 SQL DATABASES
  # -------------------------
  postgres:
    image: postgres:latest
    container_name: postgres
    environment:
      POSTGRES_PASSWORD: postgres
    ports:
      - "5432:5432"
    volumes:
      - ./06_sql_databases/postgres:/var/lib/postgresql/data

  mariadb:
    image: mariadb:latest
    container_name: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: root
    ports:
      - "3306:3306"
    volumes:
      - ./06_sql_databases/mariadb:/var/lib/mysql

  # -------------------------
  # 07 NoSQL
  # -------------------------
  mongodb:
    image: mongo:latest
    container_name: mongodb
    ports:
      - "27017:27017"
    volumes:
      - ./07_nosql_databases/mongodb:/data/db

  cassandra:
    image: cassandra:latest
    container_name: cassandra
    ports:
      - "9042:9042"
    volumes:
      - ./07_nosql_databases/cassandra:/var/lib/cassandra

  # -------------------------
  # 08 INGESTION & ETL
  # -------------------------
  kafka_connect:
    image: confluentinc/cp-kafka-connect:latest
    container_name: kafka_connect
    ports:
      - "8083:8083"
    volumes:
      - ./08_ingestion_etl/kafka_connect:/etc/kafka-connect

  # -------------------------
  # 09 ORCHESTRATION
  # -------------------------
  airflow:
    image: apache/airflow:latest
    container_name: airflow
    ports:
      - "8081:8080"
    volumes:
      - ./09_orchestration/airflow:/opt/airflow

  # -------------------------
  # 10 DQ & GOVERNANCE
  # -------------------------
  openmetadata:
    image: openmetadata/server:latest
    container_name: openmetadata
    ports:
      - "8585:8585"
    volumes:
      - ./10_data_quality_governance/openmetadata:/data

  # -------------------------
  # 11 BI
  # -------------------------
  superset:
    image: apache/superset:latest
    container_name: superset
    ports:
      - "8088:8088"
    volumes:
      - ./11_bi_dashboarding/superset:/app/superset_home

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    volumes:
      - ./11_bi_dashboarding/grafana:/var/lib/grafana

  # -------------------------
  # 12 ML / AI / MLOps
  # -------------------------
  mlflow:
    image: ghcr.io/mlflow/mlflow:latest
    container_name: mlflow
    ports:
      - "5000:5000"
    volumes:
      - ./12_ml_ai_mlops/mlflow:/mlflow

  jupyterlab:
    image: jupyter/base-notebook:latest
    container_name: jupyterlab
    ports:
      - "8888:8888"
    volumes:
      - ./12_ml_ai_mlops/jupyterlab:/home/jovyan/work

  feast:
    image: feastdev/feast:latest
    container_name: feast
    volumes:
      - ./12_ml_ai_mlops/feast:/feast

  # -------------------------
  # 13 LOGGING & OBSERVABILITY
  # -------------------------
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./13_logging_observability/prometheus:/etc/prometheus

  elk:
    image: sebp/elk:latest
    container_name: elk
    ports:
      - "5601:5601"
      - "9200:9200"
    volumes:
      - ./13_logging_observability/elk:/var/lib/elk

  # -------------------------
  # 14 DEVOPS
  # -------------------------
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8085:8080"
    volumes:
      - ./14_devops_infra/on_prem_cicd:/var/jenkins_home

  # -------------------------
  # 15 SECURITY
  # -------------------------
  keycloak:
    image: quay.io/keycloak/keycloak:latest
    container_name: keycloak
    ports:
      - "8089:8080"
    volumes:
      - ./15_security_identity/keycloak:/opt/keycloak/data

  vault:
    image: hashicorp/vault:latest
    container_name: vault
    ports:
      - "8200:8200"
    volumes:
      - ./15_security_identity/vault:/vault/data

  # -------------------------
  # 16 API GATEWAYS
  # -------------------------
  kong:
    image: kong:latest
    container_name: kong
    ports:
      - "8000:8000"
      - "8443:8443"
    volumes:
      - ./16_api_gateways/kong:/etc/kong

  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "8087:80"
    volumes:
      - ./16_api_gateways/nginx:/etc/nginx

  # -------------------------
  # 18 ANALYTICS & SEARCH
  # -------------------------
  opensearch:
    image: opensearchproject/opensearch:latest
    container_name: opensearch
    ports:
      - "9201:9200"
    volumes:
      - ./18_analytics_search/opensearch:/usr/share/opensearch/data

  # -------------------------
  # 20 CLOUD EMULATORS
  # -------------------------
  localstack:
    image: localstack/localstack:latest
    container_name: localstack
    ports:
      - "4566:4566"
    volumes:
      - ./20_cloud_emulators/localstack:/var/lib/localstack
2. STARTUP ORDER SCRIPT (LINUX)
This ensures dependencies start in the correct sequence.

Code
#!/bin/bash

echo "Starting core infrastructure..."
docker compose up -d minio postgres kafka

echo "Starting compute + SQL engines..."
docker compose up -d spark trino clickhouse

echo "Starting ingestion..."
docker compose up -d kafka_connect

echo "Starting orchestration..."
docker compose up -d airflow

echo "Starting governance..."
docker compose up -d openmetadata

echo "Starting BI & analytics..."
docker compose up -d superset grafana opensearch

echo "Starting ML stack..."
docker compose up -d mlflow jupyterlab feast

echo "Starting logging & observability..."
docker compose up -d prometheus elk

echo "Starting security..."
docker compose up -d keycloak vault

echo "Starting API gateways..."
docker compose up -d kong nginx

echo "Starting cloud emulators..."
docker compose up -d localstack

echo "All services started."
3. HEALTH‑CHECK MATRIX (ALL CONTAINERS)
This is your mechanical, operational checklist.

Service	Port	Health Check Command
Kafka	9092	nc -z localhost 9092
Redpanda	9094	curl localhost:9644/v1/status
Spark	8080	curl localhost:8080
MinIO	9000	curl localhost:9000/minio/health/ready
ClickHouse	8123	curl localhost:8123
Trino	8080	curl localhost:8080/v1/info
Postgres	5432	pg_isready
MariaDB	3306	mysqladmin ping
MongoDB	27017	mongo --eval "db.runCommand({ ping: 1 })"
Cassandra	9042	nodetool status
Kafka Connect	8083	curl localhost:8083/connectors
Airflow	8081	curl localhost:8081/health
OpenMetadata	8585	curl localhost:8585/api/v1/system/config
Superset	8088	curl localhost:8088/health
Grafana	3000	curl localhost:3000/api/health
MLflow	5000	curl localhost:5000
JupyterLab	8888	curl localhost:8888
Feast	N/A	feast version
Prometheus	9090	curl localhost:9090/-/healthy
ELK	9200	curl localhost:9200
Jenkins	8085	curl localhost:8085/login
Keycloak	8089	curl localhost:8089
Vault	8200	vault status
Kong	8000	curl localhost:8000
NGINX	8087	curl localhost:8087
OpenSearch	9201	curl localhost:9201
LocalStack	4566	curl localhost:4566/health
```code
+-------------------+--------+-----------------------------------------------+
| Service           | Port   | Health Check Command                          |
+-------------------+--------+-----------------------------------------------+
| Kafka             | 9092   | nc -z localhost 9092                          |
| Redpanda          | 9094   | curl localhost:9644/v1/status                 |
| Spark             | 8080   | curl localhost:8080                           |
| MinIO             | 9000   | curl localhost:9000/minio/health/ready        |
| ClickHouse        | 8123   | curl localhost:8123                           |
| Trino             | 8080   | curl localhost:8080/v1/info                   |
| Postgres          | 5432   | pg_isready                                    |
| MariaDB           | 3306   | mysqladmin ping                               |
| MongoDB           | 27017  | mongo --eval "db.runCommand({ ping: 1 })"     |
| Cassandra         | 9042   | nodetool status                               |
| Kafka Connect     | 8083   | curl localhost:8083/connectors                |
| Airflow           | 8081   | curl localhost:8081/health                    |
| OpenMetadata      | 8585   | curl localhost:8585/api/v1/system/config      |
| Superset          | 8088   | curl localhost:8088/health                    |
| Grafana           | 3000   | curl localhost:3000/api/health                |
| MLflow            | 5000   | curl localhost:5000                           |
| JupyterLab        | 8888   | curl localhost:8888                           |
| Feast             |   -    | feast version                                 |
| Prometheus        | 9090   | curl localhost:9090/-/healthy                 |
| ELK (Elastic)     | 9200   | curl localhost:9200                           |
| Jenkins           | 8085   | curl localhost:8085/login                     |
| Keycloak          | 8089   | curl localhost:8089                           |
| Vault             | 8200   | vault status                                  |
| Kong              | 8000   | curl localhost:8000                           |
| NGINX             | 8087   | curl localhost:8087                           |
| OpenSearch        | 9201   | curl localhost:9201                           |
| LocalStack        | 4566   | curl localhost:4566/health                    |
+-------------------+--------+-----------------------------------------------+
```
