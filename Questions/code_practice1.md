1. READ RAW FILE
python
df_raw = (
    spark.read
        .text("<file_location>Customers.csv")
        .withColumnRenamed("value", "raw_line")
)
Technical Highlights
spark.read.text()

Reads the file as raw text, not CSV.

Every line becomes a single column named value.

This is the correct approach when the file is corrupted or inconsistent.

withColumnRenamed("value", "raw_line")

Renames the default column to something meaningful.

Keeps the pipeline readable.

2. NORMALIZE DELIMITERS → ALWAYS SEMICOLON
python
df_norm = (
    df_raw
        .withColumn("step1", F.regexp_replace("raw_line", r"\t", ";"))
        .withColumn("step2", F.regexp_replace("step1", r"([A-Za-z]+ \d{1,2}), (\d{4})", r"\1@@\2"))
        .withColumn("step3", F.regexp_replace("step2", r",", ";"))
        .withColumn("step4", F.regexp_replace("step3", r"@@", ", "))
        .withColumn("step5", F.regexp_replace("step4", r"[^\x00-\x7F]", ""))
        .withColumn("step6", F.regexp_replace("step5", r";{2,}", ";"))
        .withColumn("clean_line", F.trim("step6"))
        .select("clean_line")
)
Technical Highlights
regexp_replace

Used repeatedly to sanitize and standardize the line.

step1: Replace tabs with semicolons

Handles TSV-like corruption.

step2: Temporarily protect date commas

Pattern: "Month DD, YYYY"

Replaces comma with @@ so it won’t be mistaken for a delimiter.

step3: Replace all commas with semicolons

Converts CSV → semicolon-separated.

step4: Restore date comma

Converts @@ back to ,.

step5: Remove non-ASCII characters

Cleans emojis, accents, corrupted bytes.

step6: Collapse multiple semicolons

Fixes missing fields that produce ;;;.

trim()

Removes leading/trailing whitespace.

This block normalizes any corrupted delimiter scenario.

3. SPLIT INTO FIELDS SAFELY
python
df_split = df_norm.withColumn("fields", F.split("clean_line", ";"))
Technical Highlights
split()

Converts the cleaned line into an array of fields.

Avoids schema inference errors.

python
df_final = df_split.select(
    F.expr("get(fields, 0)").alias("customer_id"),
    ...
)
Technical Highlights
F.expr("get(fields, n)")

Safely extracts array elements.

Avoids out-of-bound errors (returns null instead).

python
df_final = df_final.filter(F.col("customer_id") != "customer_id")
Technical Highlights
Removes header row by comparing the literal string "customer_id".

4. CLEAN + NORMALIZE ALL COLUMNS
python
.withColumn("email_clean", F.lower(F.trim(F.regexp_replace("email", r"[^A-Za-z0-9@._-]", ""))))
Technical Highlights
regexp_replace removes illegal characters.

trim removes whitespace.

lower standardizes case.

python
.withColumn("phone_clean", F.regexp_replace("phone", r"[^0-9]", ""))
Strips everything except digits.

python
.withColumn("country_clean", F.upper(F.trim("country")))
Standardizes country codes.

python
.withColumn("status_clean", F.lower(F.trim("status")))
Normalizes status values.

Multi-format date parsing
```python
.withColumn(
    "date_clean",
    F.coalesce(
        F.try_to_date("created_date", "yyyy-MM-dd"),
        F.try_to_date("created_date", "yyyy/MM/dd"),
        F.try_to_date("created_date", "dd/MM/yyyy"),
        F.try_to_date("created_date", "MM-dd-yy"),
        F.try_to_date("created_date", "MMMM dd, yyyy")
    )
)
```
Technical Highlights
try_to_date

Attempts parsing without throwing errors.

Returns null if format doesn’t match.

coalesce

Picks the first successful parse.

Supports multi-format corrupted date fields.

5. SURVIVORSHIP SCORING
```python
.withColumn("score_email", F.when(F.col("email_clean").contains("@"), 1).otherwise(0))
```
Valid email → score 1.

```python
.withColumn("score_phone", F.when(F.length("phone_clean") >= 10, 1).otherwise(0))
```
Valid phone → score 1.

```python
.withColumn("score_date", F.when(F.col("created_date").isNotNull(), 1).otherwise(0))
```
Valid date → score 1.

```python
.withColumn("score_status", F.when(F.col("status_clean").isin("active", "inactive"), 1).otherwise(0))
```
Valid status → score 1.

```python
.withColumn(
    "survivor_score",
    F.col("score_email") +
    F.col("score_phone") +
    F.col("score_date") +
    F.col("score_status")
)
```
Technical Highlights
Creates a composite quality score.

Higher score = better record.

This is MDM-style survivorship logic.

6. DEDUPE → KEEP BEST ROW PER CUSTOMER
```python
w = Window.partitionBy("customer_id").orderBy(F.col("survivor_score").desc())
```
Technical Highlights
Window partition groups by customer.

Order by score DESC ensures best record is first.

```python
df_survivor = (
    df_scored
        .withColumn("rank", F.row_number().over(w))
        .filter("rank = 1")
)
```
row_number() picks the top-scoring row.

Removes duplicates based on survivorship.

This is gold-standard deduplication.

7. RESEQUENCE CUSTOMER_ID
```python
w2 = Window.orderBy(F.monotonically_increasing_id())
```
Technical Highlights
monotonically_increasing_id()

Generates a unique, increasing value.

Used only for ordering, not for IDs.

```python
.withColumn("seq", F.row_number().over(w2))
.withColumn("customer_id", F.concat(F.lit("C"), F.lpad("seq", 3, "0")))
```
Technical Highlights
row_number() assigns sequential numbers.

lpad ensures fixed width (e.g., 001, 002).

concat builds IDs like C001.

This creates clean, standardized surrogate keys.

8. WRITE SILVER TO DELTA
```python
df_silver.write.format("delta").mode("overwrite").save("<output_location>silver/customers")
```
Technical Highlights
format("delta")

Writes to Delta Lake (ACID, versioned).

mode("overwrite")

Replaces existing table.

save(path)

Writes to storage (DBFS, ADLS, S3, etc.).

This is the Silver layer output.

FINAL SUMMARY — WHAT THIS PIPELINE ACHIEVES
Stage	Purpose
Read raw	Accept corrupted input safely
Normalize	Fix delimiters, encoding, ASCII, spacing
Split	Convert line → structured fields
Clean	Standardize email, phone, country, status, dates
Score	Evaluate record quality
Deduplicate	Keep best version per customer
Resequence	Generate clean surrogate keys
Write	Produce Silver Delta table
This is a full MDM-lite cleansing pipeline.
