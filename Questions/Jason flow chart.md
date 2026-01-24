```mermaid
flowchart TD
    A[One object per line] -->|NDJSON| B[multiLine=False]
    C[Starts with 

\[ \]

] -->|JSON Array| D[multiLine=True]
    E[Multi-line, no 

\[ \]

] -->|Pretty JSON| F[multiLine=True]
    G[Has comments] --> H[Strip → multiLine=True]
    I[Trailing commas] --> J[Clean → multiLine=True]
    K[Blank lines] --> L[Filter → NDJSON]
    M[Mixed formats] --> N[Normalize → NDJSON]
    O[Nested arrays] --> P[multiLine=True → flatten]
    Q[Corrupted] --> R[Fix RAW first]

```
