```code
IF file has one JSON object per line → NDJSON → multiLine=False
IF file starts with [ → JSON Array → multiLine=True
IF file spans multiple lines but no [ → Pretty JSON → multiLine=True
IF file has comments → strip → then multiLine=True
IF file has trailing commas → clean → then multiLine=True
IF file has blank lines → filter → then NDJSON
IF file is mixed → normalize → then NDJSON
IF file is nested → multiLine=True → flatten
IF file is corrupted → fix RAW first
This is the entire universe of JSON ingestion patterns for RAW → Bronze.
```

```mermaid
flowchart TD
    A[One object per line] -->|NDJSON| B[multiLine = False]
    C[Starts with array symbol] -->|JSON Array| D[multiLine = True]
    E[Multi-line object] -->|Pretty JSON| F[multiLine = True]
    G[Has comments] --> H[Strip then multiLine = True]
    I[Trailing commas] --> J[Clean then multiLine = True]
    K[Blank lines] --> L[Filter then NDJSON]
    M[Mixed formats] --> N[Normalize then NDJSON]
    O[Nested arrays] --> P[multiLine = True then flatten]
    Q[Corrupted JSON] --> R[Fix RAW first]

```

```mermaid
stateDiagram-v2
    [*] --> RAW
    RAW --> CLEAN : Validate format
    CLEAN --> INGEST : Apply correct read strategy
    INGEST --> BRONZE : Persist as Delta or Parquet
    BRONZE --> [*]
```

```mermaid
classDiagram
    class RawJSON {
        +string raw_line
        +string json_type
        +string source_path
    }

    class BronzeTable {
        +string id
        +string name
        +string constructorRef
        +string nationality
        +string url
    }

    RawJSON --> BronzeTable : transforms into
```

```mermaid
sequenceDiagram
    participant User
    participant Notebook
    participant Storage
    participant Spark
    participant BronzeDelta

    User->>Notebook: Run ingestion notebook
    Notebook->>Storage: Read RAW JSON
    Storage-->>Notebook: Return file stream
    Notebook->>Spark: Apply read strategy
    Spark->>Spark: Parse and normalize
    Spark->>BronzeDelta: Write Bronze table
    BronzeDelta-->>User: Bronze ready
```
