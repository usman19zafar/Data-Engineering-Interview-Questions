```code
                 ┌───────────────────────────────┐
                 │        Spark SQL Engine        │
                 │  (queries, tables, views)      │
                 └───────────────┬───────────────┘
                                 │
                                 │ uses metadata
                                 ▼
                 ┌────────────────────────────────┐
                 │        Hive Metastore          │
                 │  (table name, schema, format,  │
                 │   file location, view logic)   │
                 └───────────────┬────────────────┘
                                 │
                                 │ points to files
                                 ▼
        ┌──────────────────────────────────────────────────────┐
        │                     ADLS / Data Lake                  │
        │  (actual data files: CSV, JSON, Parquet, Delta)       │
        └──────────────────────────────────────────────────────┘

                 ┌───────────────────────────────┐
                 │        Hive Metastore          │
                 └───────────────┬───────────────┘
                                 │
                 ┌───────────────┴───────────────┐
                 │                               │
        MANAGED TABLE                     EXTERNAL TABLE
        (Spark owns data)                 (You own data)
                 │                               │
                 ▼                               ▼
        ┌──────────────────┐           ┌──────────────────┐
        │  Data Location   │           │  Data Location   │
        │  decided by      │           │  YOU specify     │
        │  Spark           │           │  the path        │
        └──────────────────┘           └──────────────────┘

DROP TABLE:
- Managed → deletes metadata + files  
- External → deletes metadata only  
```

```code
Databricks Workspace
        │
        ▼
┌───────────────────────────────┐
│        DATABASE (schema)      │
│   - groups tables & views     │
└───────────────┬──────────────┘
                │
   ┌────────────┴────────────┐
   │                         │
TABLES                    VIEWS
- backed by files         - backed by SQL
- stored in ADLS          - no data stored
- registered in HMS       - registered in HMS
```

```code
        BRONZE (raw files)
                │
                ▼
        SILVER (cleaned files)
                │
                ▼
        GOLD (tables)
                │
                ▼
        Hive Metastore
                │
                ▼
        Spark SQL / BI Tools
```

