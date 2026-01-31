1. LOAD GOLD DIMENSIONS + GOLD ORDERS
python
orders = spark.read.format("delta").load("<input_path>gold/orders")
customers = spark.read.format("delta").load("<input_path>gold/customers")
products = spark.read.format("delta").load("<input_path>gold/products")
Technical Highlights
Reads Gold‑layer tables for Orders, Customers, Products.

Delta Lake ensures:

ACID transactions

schema metadata

version history

No schema inference required — Delta handles it.

This is the correct starting point for building a fact table.

2. JOIN LOGIC (Orders = fact driver)
python
df = (
    orders.alias("o")
    .join(customers.alias("c"), F.col("o.customer_id") == F.col("c.customer_id"), "left")
    .join(products.alias("p"), F.col("o.product_code") == F.col("p.product_code"), "left")
)
Technical Highlights
orders.alias("o")
Orders is the fact driver.

Every row in the fact table originates from Orders.

LEFT JOIN customers
python
.join(customers.alias("c"), o.customer_id == c.customer_id, "left")
Ensures:

All orders remain in the fact table.

Missing customers → NULL dimension fields.

This is correct for slowly changing or missing dimension rows.

LEFT JOIN products
python
.join(products.alias("p"), o.product_code == p.product_code, "left")
Enriches orders with product attributes.

Missing product rows → NULL dimension fields.

Correct for late-arriving or missing product dimension records.

3. SELECT FINAL FACT COLUMNS
python
df = df.select(
    F.col("o.order_id"),
    F.col("o.customer_id"),
    F.col("p.product_id"),
    F.col("o.product_code"),
    F.col("p.product_name"),
    F.col("p.category"),
    F.col("o.quantity"),
    F.col("p.price"),
    F.col("o.total_amount"),
    F.col("o.order_date"),
    F.col("o.status"),
    F.current_timestamp().alias("ingestion_date")
)
Technical Highlights
Fact keys
order_id → primary fact key

customer_id → foreign key to Customer dimension

product_id → foreign key to Product dimension

Degenerate dimension
product_code is kept as a degenerate dimension attribute.

Dimension attributes
product_name, category, price

These come from the Product dimension.

Fact measures
quantity

total_amount

These are the core numeric measures for BI.

Fact timestamps
order_date → business event timestamp

ingestion_date → pipeline audit timestamp

Fact status
status → order lifecycle attribute (e.g., completed, cancelled)

4. WRITE GOLD FACT
python
df.write.format("delta").mode("overwrite").save("/<output_path>/gold/sales_fact")
Technical Highlights
Writes the final fact table in Delta Lake.

overwrite ensures:

idempotent runs

deterministic output

no duplicates

Produces a clean, conformed, analytics‑ready fact table.

FINAL MECHANICAL SUMMARY
Stage	Purpose
Load Gold dims	Bring in clean, validated dimension tables
Join with Orders	Build a conformed fact table
Select final columns	Define fact keys, measures, attributes
Add ingestion_date	Add audit + lineage metadata
Write Gold Fact	Produce final Sales Fact table
This is a proper star‑schema fact table build:

Orders = fact driver

Customers = dimension

Products = dimension

Measures = quantity, amount

Attributes = product/category/status

Timestamps = order_date + ingestion_date
