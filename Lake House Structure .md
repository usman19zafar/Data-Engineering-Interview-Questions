1. High‑level architecture
```text
                F1 LAKEHOUSE (LOGICAL VIEW)

        RAW (Blob, CSV/JSON as‑is)
                │
                ▼
        BRONZE (Parquet, minimal structure)
                │
                ▼
        SILVER (Clean, typed, modeled)
                │
                ▼
        GOLD (Business-ready, dimensional, aggregated)
                │
                ▼
        BI / ML (Power BI, dashboards, models)
```
Raw: source files, never modified

Bronze: “as‑is but Parquet”

Silver: cleaned, typed, standardized

Gold: business logic, joins, star schema

You’ve already nailed the schema engineering for circuits—that’s the template.
_______________________________________________________________________________________________________________________________________________________________________________________
2. Raw layer (Blob Storage, WASBS)
Location examples:
```code
wasbs://raw@<container name>.core.windows.net/<File name>.csv
```
Purpose:

immutable source of truth

no schema enforcement

no transformations

_______________________________________________________________________________________________________________________________________________________________________________________
3. Bronze layer (Parquet, minimal logic)
Pattern (per dataset):

```python
bronze_path = "wasbs://bronze@uzi786.blob.core.windows.net/<entity>"

df_bronze = spark.read \
    .option("header", True) \
    .csv(raw_path)  # or .json()

df_bronze.write \
    .mode("overwrite") \
    .parquet(bronze_path)
```
Bronze tables:

bronze.circuits

bronze.races

bronze.constructors

bronze.drivers

bronze.results

bronze.pit_stops

bronze.lap_times

Role:  
Just get it in, in a consistent, query‑friendly format.

_______________________________________________________________________________________________________________________________________________________________________________________
4. Silver layer (clean, typed, modeled)
Here’s where your schema work pays off.

4.1 Silver circuits (you’re already here)
explicit schema

rename columns

drop url

add ingestion_date

```python
from pyspark.sql.functions import current_timestamp, col

circuits_silver = circuits_df \
    .withColumnRenamed("circuitId",  "circuit_id") \
    .withColumnRenamed("circuitRef", "circuit_ref") \
    .withColumnRenamed("lat",        "latitude") \
    .withColumnRenamed("lng",        "longitude") \
    .withColumnRenamed("alt",        "altitude") \
    .drop("url") \
    .withColumn("ingestion_date", current_timestamp())
```
Write to:

wasbs://silver@<container name>.core.windows.net/circuits

4.2 Silver races
parse date + time into race_timestamp

rename columns

enforce types

4.3 Silver drivers
nested schema (StructType inside StructType)

split name fields

enforce date types

4.4 Silver results, constructors, pit_stops, lap_times
enforce foreign keys (race_id, driver_id, constructor_id)

cast numeric fields (positions, points, laps, times)

add ingestion_date everywhere

Role of Silver:  
Everything is clean, typed, relational, and join‑ready.

_______________________________________________________________________________________________________________________________________________________________________________________
5. Gold layer (dimensional model)
This is where the business story lives.

5.1 Dimensions
gold.dim_circuits

gold.dim_races

gold.dim_drivers

gold.dim_constructors

Each is a narrow, descriptive table:

```python
dim_circuits = circuits_silver.select(
    "circuit_id",
    "circuit_ref",
    "name",
    "location",
    "country",
    "latitude",
    "longitude",
    "altitude"
)
```
5.2 Facts
gold.fact_results

grain: one row per driver per race

keys: race_id, driver_id, constructor_id

measures: points, position, laps, time, fastest_lap

gold.fact_pit_stops

grain: one row per pit stop per driver per race

gold.fact_lap_times

grain: one row per lap per driver per race

These are built by joining Silver tables:

```python
fact_results = results_silver \
    .join(races_silver, "race_id") \
    .join(drivers_silver, "driver_id") \
    .join(constructors_silver, "constructor_id")
```
Write to:

wasbs://gold@<Container name>.windows.net/fact_results

etc.

_______________________________________________________________________________________________________________________________________________________________________________________
6. Serving layer (Power BI / SQL)
From Gold, you can:

connect Power BI to Serverless SQL / external tables

build measures like:

total points by driver

wins by constructor

performance by circuit

year‑over‑year trends

Gold is read‑optimized, business‑friendly, and stable.

_______________________________________________________________________________________________________________________________________________________________________________________
7. ASCII lineage view (end‑to‑end)
```text
RAW (Blob, CSV/JSON)
    circuits.csv, races.csv, drivers.json, ...
        │
        ▼
BRONZE (Parquet, as‑is)
    bronze.circuits, bronze.races, ...
        │
        ▼
SILVER (Clean, typed, modeled)
    silver.circuits
    silver.races
    silver.drivers
    silver.results
    silver.pit_stops
    silver.lap_times
        │
        ▼
GOLD (Dimensional, business-ready)
    dim_circuits
    dim_races
    dim_drivers
    dim_constructors
    fact_results
    fact_pit_stops
    fact_lap_times
        │
        ▼
SERVE
    Power BI, dashboards, ML, APIs
```

_______________________________________________________________________________________________________________________________________________________________________________________
8. What your schema work unlocked
Because you did the schema exercise properly, you’re now ready to:

standardize all Silver tables the same way

build consistent Gold facts and dims

plug Power BI directly into Gold

talk about this as a real Lakehouse project on your CV/GitHub

If you want next, we can:

design the exact Gold fact_results table (columns + logic)

or write one full notebook per layer (Bronze/Silver/Gold) for a single entity like results.
