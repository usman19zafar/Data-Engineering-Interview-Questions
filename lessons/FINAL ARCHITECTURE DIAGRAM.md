```code
                           ┌──────────────────────────────┐
                           │        API GATEWAY           │
                           │     Kong + NGINX + Keycloak  │
                           └───────────────┬──────────────┘
                                           │
                                           ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│                              SECURITY & IDENTITY                             │
│                         Keycloak (IAM) + Vault (Secrets)                     │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│                           STREAMING & INGESTION                               │
│   Kafka  ───►  Kafka Connect  ───►  Spark Structured Streaming                │
│   (Redpanda optional)                                                         │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│                               DATA LAKEHOUSE                                 │
│   MinIO (RAW → BRONZE → SILVER → GOLD)                                       │
│   Spark (ETL + ML)                                                           │
│   Trino (SQL)                                                                │
│   ClickHouse (OLAP)                                                          │
│   Postgres / MariaDB (SQL DBs)                                               │
│   MongoDB / Cassandra (NoSQL DBs)                                            │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│                         DATA QUALITY & GOVERNANCE                             │
│         Great Expectations (DQ) + OpenMetadata (Governance + Lineage)        │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│                           ML / AI / MLOps                                     │
│   JupyterLab  │  MLflow  │  Feast  │  Spark MLlib                             │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│                         BI, ANALYTICS & SEARCH                                │
│   Superset (BI)  │  Grafana (Metrics)  │  OpenSearch (Search)                 │
│   ClickHouse (Analytics)                                                    │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│                         LOGGING & OBSERVABILITY                               │
│   Prometheus (Metrics)  │  Grafana (Dashboards)                               │
│   ELK Stack (Logs)  │  OpenSearch (Log Search)                                │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│                         ORCHESTRATION & DEVOPS                                │
│   Airflow (Orchestration)                                                     │
│   GitHub Actions (CI/CD)  │  On‑prem CI/CD (Jenkins optional)                 │
│   Docker Compose (Runtime)  │  Terraform (IaC)                                │
└──────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│                               CLOUD EMULATORS                                 │
│   LocalStack (AWS)  │  MinIO (S3)  │  Firestore Emulator (GCP)                │
└──────────────────────────────────────────────────────────────────────────────┘
```

CANONICAL FOLDER STRUCTURE (ALL 20 DOMAINS)
This is the official, final, zero‑drift folder structure for your repo.
Every domain is represented. Every tool has a home.
This is the structure you will use in your Data Architect Workbook.\

```code
/platform
│
├── 01_streaming/
│   ├── kafka/
│   └── redpanda/
│
├── 02_stream_processing/
│   └── spark_streaming/
│
├── 03_data_lake/
│   └── minio/
│
├── 04_olap/
│   └── clickhouse/
│
├── 05_sql_engines/
│   ├── trino/
│   └── spark_sql/
│
├── 06_sql_databases/
│   ├── postgres/
│   └── mariadb/
│
├── 07_nosql_databases/
│   ├── mongodb/
│   └── cassandra/
│
├── 08_ingestion_etl/
│   ├── kafka_connect/
│   └── spark_jobs/
│
├── 09_orchestration/
│   └── airflow/
│
├── 10_data_quality_governance/
│   ├── great_expectations/
│   └── openmetadata/
│
├── 11_bi_dashboarding/
│   ├── superset/
│   └── grafana/
│
├── 12_ml_ai_mlops/
│   ├── mlflow/
│   ├── jupyterlab/
│   ├── feast/
│   └── spark_ml/
│
├── 13_logging_observability/
│   ├── prometheus/
│   ├── grafana/
│   └── elk/
│
├── 14_devops_infra/
│   ├── github_actions/
│   ├── on_prem_cicd/
│   ├── docker/
│   └── terraform/
│
├── 15_security_identity/
│   ├── keycloak/
│   └── vault/
│
├── 16_api_gateways/
│   ├── kong/
│   ├── nginx/
│   ├── keycloak_integration/
│   └── vault_integration/
│
├── 17_message_brokers_alt/
│   └── kafka/
│
├── 18_analytics_search/
│   ├── opensearch/
│   └── clickhouse/
│
├── 19_file_object_storage/
│   ├── minio/
│   └── local_fs/
│
└── 20_cloud_emulators/
    ├── localstack/
    ├── minio/
    └── firestore_emulator/
```


