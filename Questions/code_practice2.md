1. READ RAW FILE
python
df_raw = (
    spark.read
        .text("<file_location>.csv")
        .withColumnRenamed("value", "raw_line")
)
Technical Highlights
spark.read.text()

Reads the file line‑by‑line, no schema inference.

Every line becomes a single string column named value.

This is the correct approach for corrupted or inconsistent CSVs.

withColumnRenamed("value", "raw_line")

Makes the column semantically meaningful.

Keeps the pipeline readable.

2. NORMALIZE DELIMITERS → ALWAYS SEMICOLON
python
df_norm = (
    df_raw
        .withColumn("step1", F.regexp_replace("raw_line", r"\t", ";"))
        .withColumn("step2", F.regexp_replace("step1", r",", ";"))
        .withColumn("step3", F.regexp_replace("step2", r";{2,}", ";"))
        .withColumn("step4", F.regexp_replace("step3", r"[^\x00-\x7F]", ""))
        .withColumn("clean_line", F.trim("step4"))
        .select("clean_line")
)
Technical Highlights
step1: Replace tabs → semicolons

Handles TSV or mixed-delimiter corruption.

step2: Replace commas → semicolons

Forces a single delimiter across the file.

step3: Collapse multiple semicolons

Fixes missing fields that produce ;;;.

step4: Remove non‑ASCII characters

Strips emojis, accents, corrupted bytes.

trim: Remove leading/trailing whitespace.

select("clean_line")

Drops intermediate columns, keeps pipeline clean.

This block standardizes all rows into a safe, predictable format.

3. SPLIT INTO FIELDS SAFELY
python
df_split = df_norm.withColumn("fields", F.split("clean_line", ";"))
Technical Highlights
split() converts the cleaned line into an array of fields.

Avoids schema inference errors.

Handles rows with missing fields gracefully.

python
df_final = df_split.select(
    F.expr("get(fields, 0)").alias("order_id"),
    ...
)
Technical Highlights
get(fields, n)

Safely extracts array elements.

Returns null if index is missing → avoids exceptions.

python
df_final = df_final.filter(F.col("order_id") != "order_id")
Technical Highlights
Removes header row by comparing literal string "order_id".

4. CLEAN + NORMALIZE ALL COLUMNS
customer_id
python
.withColumn(
    "customer_id",
    F.when(F.col("customer_id").rlike("^C\\d{3}$"), F.col("customer_id")).otherwise(None)
)
Technical Highlights
Enforces strict pattern: C001, C123, etc.

Invalid IDs → set to NULL.

product_code
python
.withColumn("product_code", F.upper(F.regexp_replace("product_code", r"[^A-Za-z]", "")))
Removes all non‑letters.

Converts to uppercase.

python
.withColumn("product_code", F.when(F.trim("product_code") == "", None).otherwise(F.col("product_code")))
Empty strings → NULL.

total_amount
python
.withColumn("total_amount", F.regexp_replace("total_amount", r"[^0-9.]", ""))
Strips currency symbols, commas, garbage.

python
.withColumn("total_amount", F.expr("try_cast(total_amount AS double)"))
try_cast prevents pipeline failure.

Invalid numbers → NULL.

order_date
python
.withColumn(
    "order_date_clean",
    F.coalesce(
        F.try_to_date("order_date", "yyyy-MM-dd"),
        F.try_to_date("order_date", "yyyy/MM/dd"),
        F.try_to_date("order_date", "dd/MM/yyyy"),
        F.try_to_date("order_date", "MM-dd-yy"),
        F.try_to_date("order_date", "MMMM dd, yyyy")
    )
)
Technical Highlights
Attempts multiple date formats.

coalesce picks the first successful parse.

Handles corrupted or inconsistent date formats.

status
python
.withColumn(
    "status",
    F.lower(F.trim(F.regexp_replace("status", r"[^A-Za-z]", "")))
)
Technical Highlights
Removes non‑letters.

Standardizes to lowercase.

Ensures consistent downstream filtering.

5. RESEQUENCE ORDER_ID (ALWAYS START AT O001)
python
df_tmp = df_clean.withColumn("dummy", F.lit(1))
Creates a single partition for ordering.

python
w = Window.partitionBy("dummy").orderBy(F.monotonically_increasing_id())
Technical Highlights
monotonically_increasing_id()

Generates unique, increasing values.

Used only for ordering, not for business keys.

python
.withColumn("seq", F.row_number().over(w))
Assigns sequential numbers starting at 1.

python
.withColumn("order_id", F.concat(F.lit("O"), F.lpad("seq", 3, "0")))
Technical Highlights
Builds surrogate keys:

O001, O002, O003, …

6. WRITE SILVER TO DELTA
python
df_silver.write.format("delta").mode("overwrite").save("<output_location>orders")
Technical Highlights
Writes to Delta Lake (ACID, versioned).

overwrite ensures idempotent runs.

Produces a clean Silver layer table.

FINAL MECHANICAL SUMMARY
Stage	Purpose
Read raw	Accept corrupted input safely
Normalize	Fix delimiters, ASCII, spacing
Split	Convert line → structured fields
Clean	Standardize IDs, product codes, dates, amounts
Validate	Remove invalid patterns
Resequence	Generate clean surrogate keys
Write	Produce Silver Delta table
This is a production‑grade cleansing pipeline with:

delimiter normalization

schema enforcement

multi‑format date parsing

safe casting

surrogate key generation

Silver‑layer output
