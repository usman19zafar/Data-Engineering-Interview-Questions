1. READ RAW FILE
python
df_raw = (
    spark.read
        .text("<file_location>products.csv")
        .withColumnRenamed("value", "raw_line")
)
Technical Highlights
spark.read.text()

Reads the file line‑by‑line, no schema inference.

Every row becomes a single string column.

This is the correct approach for corrupted CSVs.

withColumnRenamed("value", "raw_line")

Makes the column meaningful for downstream transformations.

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

Forces a single delimiter across the entire file.

step3: Collapse multiple semicolons

Fixes missing fields that produce ;;;.

step4: Remove non‑ASCII characters

Removes emojis, accents, corrupted bytes.

trim: Remove leading/trailing whitespace.

select("clean_line")

Drops intermediate columns to keep the pipeline clean.

This block standardizes all rows into a safe, predictable format.

3. SPLIT INTO FIELDS SAFELY
python
df_split = df_norm.withColumn("fields", F.split("clean_line", ";"))
Technical Highlights
split() converts the cleaned line into an array of fields.

Handles missing fields gracefully.

Avoids schema inference errors.

python
df_final = df_split.select(
    F.expr("get(fields, 0)").alias("product_id"),
    ...
)
Technical Highlights
get(fields, n)

Safely extracts array elements.

Returns NULL if index is missing → avoids exceptions.

python
df_final = df_final.filter(F.col("product_id") != "product_id")
Technical Highlights
Removes header row by comparing literal string "product_id".

4. CLEAN + NORMALIZE ALL COLUMNS
product_id
python
.withColumn(
    "product_id",
    F.when(F.col("product_id").rlike("^P\\d{3}$"), F.col("product_id")).otherwise(None)
)
Technical Highlights
Enforces strict pattern: P001, P123, etc.

Invalid IDs → set to NULL.

product_code
python
.withColumn("product_code", F.upper(F.regexp_replace("product_code", r"[^A-Za-z0-9]", "")))
Removes all non‑alphanumeric characters.

Converts to uppercase.

python
.withColumn("product_code", F.when(F.trim("product_code") == "", None).otherwise(F.col("product_code")))
Empty strings → NULL.

product_name
python
.withColumn(
    "product_name",
    F.trim(F.regexp_replace("product_name", r"[^A-Za-z0-9_ ]", ""))
)
Technical Highlights
Removes illegal characters.

Preserves letters, digits, underscores, spaces.

Ensures clean, display‑safe product names.

category
python
.withColumn(
    "category",
    F.trim(F.regexp_replace("category", r"[^A-Za-z]", ""))
)
Technical Highlights
Removes digits, symbols, punctuation.

Leaves only alphabetic characters.

Standardizes category labels.

price
python
.withColumn("price", F.regexp_replace("price", r"[^0-9.]", ""))
Strips currency symbols, commas, garbage.

python
.withColumn("price", F.expr("try_cast(price AS double)"))
Technical Highlights
try_cast prevents pipeline failure.

Invalid numbers → NULL.

Ensures numeric type for downstream analytics.

status
python
.withColumn(
    "status",
    F.lower(F.trim(F.regexp_replace("status", r"[^A-Za-z]", "")))
)
Technical Highlights
Removes non‑letters.

Standardizes to lowercase.

Ensures consistent filtering (e.g., active, inactive).

5. WRITE SILVER TO DELTA
python
df_clean.write.format("delta").mode("overwrite").save("<outputlocation>")
Technical Highlights
Writes to Delta Lake (ACID, versioned).

overwrite ensures idempotent runs.

Produces a clean Silver layer table.

FINAL MECHANICAL SUMMARY
Stage	Purpose
Read raw	Accept corrupted input safely
Normalize	Fix delimiters, ASCII, spacing
Split	Convert line → structured fields
Clean	Standardize IDs, codes, names, categories, prices, status
Validate	Remove invalid patterns
Write	Produce Silver Delta table
This is a production‑grade cleansing pipeline for the Products dimension.
