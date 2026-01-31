1. LOAD GOLD SALES FACT
python
df = spark.read.format("delta").load("<output_path>/gold/sales_fact")
Technical Highlights
Reads the Gold Sales Fact table as the input source.

Delta Lake ensures:

ACID guarantees

schema metadata

version history

No schema inference required — Delta handles it.

This is the correct starting point for a Gold‑to‑Gold aggregate.

2. AGGREGATIONS (BUSINESS RULES)
python
df = df.groupBy("customer_id").agg(
    F.count("*").alias("total_orders"),
    F.sum("quantity").alias("total_quantity"),
    F.sum("total_amount").alias("total_revenue"),
    F.round(F.avg("total_amount"), 2).alias("avg_order_value"),
    F.min("order_date").alias("first_order_date"),
    F.max("order_date").alias("last_order_date")
)
Technical Highlights
groupBy("customer_id")
Aggregates all orders belonging to each customer.

Produces one row per customer.

total_orders
python
F.count("*").alias("total_orders")
Counts all order rows.

Includes all statuses unless filtered earlier in the pipeline.

total_quantity
python
F.sum("quantity").alias("total_quantity")
Sums all purchased units.

Requires quantity to be numeric (validated in Silver/Gold Orders).

total_revenue
python
F.sum("total_amount").alias("total_revenue")
Sums all order amounts.

Produces the customer’s lifetime revenue.

avg_order_value
python
F.round(F.avg("total_amount"), 2).alias("avg_order_value")
Computes AOV = revenue / number of orders.

Rounded to 2 decimals for BI dashboards.

first_order_date / last_order_date
python
F.min("order_date")
F.max("order_date")
min gives the earliest order → customer acquisition date.

max gives the most recent order → recency metric.

These are foundational for RFM modeling.

Add ingestion_date
python
df = df.withColumn("ingestion_date", F.current_timestamp())
Technical Highlights
Adds audit metadata.

Supports:

lineage

SLA tracking

debugging

incremental refresh logic

This is a Gold‑layer best practice.

3. WRITE GOLD REVENUE SUMMARY
python
df.write.format("delta").mode("overwrite").save("<output_path>/gold/revenue_summary")
Technical Highlights
Writes the aggregated table in Delta Lake.

overwrite ensures:

idempotent runs

deterministic output

no duplicates

Produces a final analytics‑ready summary table.

FINAL MECHANICAL SUMMARY
Stage	Purpose
Load Gold Fact	Start from validated, business‑clean data
Group by customer	Build customer‑level aggregates
Compute metrics	Orders, quantity, revenue, AOV, recency
Add ingestion_date	Add audit + lineage metadata
Write Gold Summary	Produce final BI‑ready table
This pipeline is a classic Gold‑layer aggregate:

No cleansing

No schema repair

Only business metrics and summarization
