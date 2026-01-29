Normal ADF Pipeline
Two words: Move data  
Business analogy: A courier picking up a package and dropping it off — no inspection, no quality check.

Mechanical truth:

Source → light transform → sink

One dataset at a time

Minimal validation

Static parameters

No medallion layering

Goal = “just move it”

2️⃣ MDM‑Style ADF Pipeline
Two words: Master data  
Business analogy: A customs checkpoint — every item is inspected, validated, standardized, and only clean, legal, deduped items enter the country.

Mechanical truth:

Multiple entities (Customers, Products, Orders)

Deduplication + standardization

Derived business fields

Mandatory validation (schema, nulls, row counts, rules)

Fully parameterized (load_date, entity_name)

Raw → Bronze → Silver → Gold

Enterprise-grade consistency
________________________________________________________________________________________________________________________________________________________

ADF Pipelines: Normal vs MDM (Workbook Block)
1️⃣ Normal ADF Pipeline
Two words: Move data  
Analogy: Courier delivery — pick up, drop off, no inspection.

Mechanical definition:

Single source → single sink

Light transforms only

Minimal validation

Static parameters

No medallion layers

Goal: transport data, not improve it

2️⃣ MDM‑Style ADF Pipeline
Two words: Master data  
Analogy: Customs checkpoint — inspect, validate, standardize, approve.

Mechanical definition:

Multiple business entities

Deduplication + standardization

Derived business fields

Mandatory quality checks (schema, nulls, row counts, rules)

Fully parameterized (load_date, entity_name)

Raw → Bronze → Silver → Gold

Goal: produce clean, trusted, enterprise‑grade data

3️⃣ Why Project on MDM
Two words: Enterprise pipeline  
Analogy: Factory line — every batch follows the same SOP.

Mechanical definition:

Same logic applied across all entities

Dedup + standardize + derive

Batch‑driven via load_date

Gold‑layer validation

Horizontally scalable (more entities)

Vertically scalable (more dates)

