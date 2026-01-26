⭐ THE EXACT CRITERIA YOU WILL USE IN YOUR PROJECT
Layer	Table Type	Why
Bronze	External	Raw files must never be deleted by Spark
Silver	External	Cleaned files still belong to your pipeline, not Spark
Gold	Managed	Final curated tables for BI, SQL, governance
This is the industry standard lakehouse pattern.

```code
┌──────────────────────────────────────────────────────────────┐
│        LAKEHOUSE TABLE DECISION CRITERIA (PROJECT USE)        │
└──────────────────────────────────────────────────────────────┘

                ┌──────────────┐
                │   BRONZE     │
                └──────┬───────┘
                       │
                       ▼
        ┌──────────────────────────────────────────────┐
        │ Table Type:  EXTERNAL                        │
        │ Why:        Raw files must NEVER be deleted  │
        │             by Spark                          │
        └──────────────────────────────────────────────┘


                ┌──────────────┐
                │   SILVER     │
                └──────┬───────┘
                       │
                       ▼
        ┌──────────────────────────────────────────────┐
        │ Table Type:  EXTERNAL                        │
        │ Why:        Cleaned files still belong to    │
        │             your pipeline, not Spark          │
        └──────────────────────────────────────────────┘


                ┌──────────────┐
                │    GOLD      │
                └──────┬───────┘
                       │
                       ▼
        ┌──────────────────────────────────────────────┐
        │ Table Type:  MANAGED                         │
        │ Why:        Final curated tables for BI, SQL │
        │             governance, catalog, lineage      │
        └──────────────────────────────────────────────┘


┌──────────────────────────────────────────────────────────────┐
│ Industry Standard Lakehouse Pattern                          │
│   • Bronze  → External                                        │
│   • Silver  → External                                        │
│   • Gold    → Managed                                         │
└──────────────────────────────────────────────────────────────┘
```
