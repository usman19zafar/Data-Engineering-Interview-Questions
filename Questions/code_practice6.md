1. LOAD SILVER
python
df = spark.read.format("delta").load("<output_path>/products")
Technical Highlights
Reads the Silver Delta table.

Delta provides:

ACID guarantees

schema metadata

versioning

No schema inference required — Delta handles it.

This is the correct starting point for a Gold transformation.

2. BUSINESS RULES
Rule: Drop invalid product_id
python
df = df.filter(F.col("product_id").isNotNull())
Technical Highlights
Enforces entity integrity.

Removes:

corrupted rows

incomplete rows

Silver leftovers that became null after cleansing

Gold must contain only valid business keys.

Rule: Clean empty strings → NULL
python
for c in df.columns:
    df = df.withColumn(c, F.when(F.trim(F.col(c)) == "", None).otherwise(F.col(c)))
Technical Highlights
Iterates over all columns.

Converts:

""

" "

" "  
→ into NULL.

This standardizes null semantics across the entire table.

Rule: Safe numeric conversion for price
python
df = df.withColumn(
    "price",
    F.expr("try_cast(price AS double)")
)
Technical Highlights
try_cast prevents pipeline failure.

Invalid numeric strings → NULL.

Ensures price is a real numeric type for BI/ML.

Rule: Drop invalid price rows
python
df = df.filter(F.col("price").isNotNull())
Technical Highlights
Removes rows where price:

failed conversion

was corrupted

was missing

Gold must contain valid, analyzable numeric values.

Rule: Round price
python
df = df.withColumn("price", F.round(F.col("price"), 2))
Technical Highlights
Standardizes currency precision.

Ensures consistent reporting in dashboards.

Rule: Status cleanup
python
df = df.withColumn(
    "status",
    F.when(F.trim("status") == "", "unknown")
     .otherwise(F.col("status"))
)
Technical Highlights
Empty or whitespace-only status → "unknown".

Prevents empty strings from polluting category filters.

Rule: Add ingestion_date
python
df = df.withColumn("ingestion_date", F.current_timestamp())
Technical Highlights
Adds audit metadata.

Supports:

lineage

SLA tracking

debugging

incremental refresh logic

This is a Gold-layer best practice.

3. WRITE GOLD
python
df.write.format("delta").mode("overwrite").save("<output_path>/gold/products")
Technical Highlights
Writes final Gold table in Delta Lake.

overwrite ensures:

idempotent runs

deterministic output

no duplicates

This produces a clean, trusted, analytics-ready Products table.

FINAL MECHANICAL SUMMARY
Stage	Purpose
Load Silver	Start from clean, standardized data
Drop invalid IDs	Enforce entity integrity
Clean empty → NULL	Standardize null semantics
Safe numeric cast	Prevent pipeline failure
Drop invalid numerics	Ensure analytic correctness
Round price	Standardize currency precision
Status cleanup	Normalize categorical values
Add ingestion_date	Add audit + lineage metadata
Write Gold	Produce final analytics-ready table
