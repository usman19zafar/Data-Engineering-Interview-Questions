1. LOAD SILVER
python
df = spark.read.format("delta").load("<input_path>/orders")
Technical Highlights
Reads the Silver Delta table.

Delta guarantees:

ACID transactions

schema enforcement

version history

No schema inference needed — Delta metadata provides it.

This is the correct starting point for a Gold transformation.

2. BUSINESS RULES
Rule: Drop invalid customer_id
python
df = df.filter(F.col("customer_id").isNotNull())
Technical Highlights
Ensures every Gold record has a valid foreign key.

Removes:

corrupted rows

incomplete rows

Silver leftovers that became null after cleansing

This enforces referential integrity.

Rule: Safe numeric conversions
python
df = df.withColumn(
    "quantity",
    F.abs(F.expr("try_cast(quantity AS double)"))
)
Technical Highlights
try_cast prevents pipeline failure.

Invalid values → NULL.

abs() ensures no negative quantities.

python
df = df.withColumn(
    "total_amount",
    F.expr("try_cast(total_amount AS double)")
)
Technical Highlights
Converts corrupted numeric strings into real numbers.

Invalid values → NULL.

Rule: Drop invalid numeric rows
python
df = df.filter(F.col("quantity").isNotNull())
df = df.filter(F.col("total_amount").isNotNull())
Technical Highlights
Removes rows where numeric conversion failed.

Ensures Gold contains only valid, analyzable metrics.

Rule: Status cleanup
python
df = df.withColumn(
    "status",
    F.when(F.trim("status") == "", "unknown")
     .otherwise(F.col("status"))
)
Technical Highlights
Empty or whitespace-only status → "unknown".

Prevents empty strings from polluting downstream dashboards.

Rule: Drop invalid dates
python
df = df.filter(F.col("order_date").isNotNull())
Technical Highlights
Ensures every Gold record has a valid business timestamp.

Removes:

corrupted dates

unparseable formats

nulls created during Silver cleansing

This is a hard business constraint.

Rule: Round amount
python
df = df.withColumn("total_amount", F.round(F.col("total_amount"), 2))
Technical Highlights
Standardizes currency precision.

Ensures consistent reporting in BI dashboards.

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
df.write.format("delta").mode("overwrite").save("<output_path>/gold/orders")
Technical Highlights
Writes final Gold table in Delta Lake.

overwrite ensures:

idempotent runs

deterministic output

no duplicates

This produces a clean, trusted, analytics-ready Orders table.

FINAL MECHANICAL SUMMARY
Stage	Purpose
Load Silver	Start from cleaned, standardized data
Drop invalid IDs	Enforce referential integrity
Safe numeric casting	Prevent pipeline failure
Drop invalid numerics	Ensure analytic correctness
Status cleanup	Standardize categorical values
Drop invalid dates	Enforce business timestamp integrity
Round amounts	Standardize currency precision
Add ingestion_date	Add audit + lineage metadata
Write Gold	Produce final analytics-ready table
This is a proper Gold-layer pipeline:

No cleansing logic

No schema repair

Only business rules, validations, and final standardization
