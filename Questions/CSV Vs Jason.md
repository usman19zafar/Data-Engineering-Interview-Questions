CSV vs JSON

One Word: Structure
Two Words: Flat vs Nested
Business Analogy:
CSV is a simple Excel sheet.
JSON is a folder with sub‑folders inside each row.

Mechanical Truth
CSV

    Flat, row‑column format
    No nested objects
    No arrays
    Schema is simple
    Easy to parse
    Good for tabular data
    Spark reads it with .csv()

JSON

    Supports nested objects
    Supports arrays
    Supports key‑value structure
    Schema can be complex
    Requires explicit schema for performance
    Spark reads it with .json()
_____________________________________________________________________________________________________________________________________________
2. DataFrame Schema: DDL vs StructType

One Word: Syntax
Two Words: Text vs Objects
Business Analogy: StructType is building a machine part‑by‑part. DDL is giving the factory a blueprint in one line.

Mechanical Truth

    StructType / StructField
    Python object
    Verbose
    Good for programmatic schema building

Example:

```python
StructType([
    StructField("id", IntegerType(), True),
    StructField("name", StringType(), True)
])
```
DDL Schema

    Text string
    Faster to write
    Cleaner for simple schemas

Example:

```python

"id INT, name STRING"
Seed Difference
StructType = object‑based schema
```

DDL = string‑based schema

Both produce the same schema internally

_____________________________________________________________________________________________________________________________________________
3. DataFrame vs DDL (conceptual difference)
One Word: Layers
Two Words: Data vs Definition
Business Analogy:
DDL is the blueprint.
DataFrame is the building constructed from the blueprint.

Mechanical Truth
DDL
Only defines column names + types

No data

Used BEFORE reading data

DataFrame
Actual data in memory

Has rows + columns

Used AFTER reading data

Seed Difference
DDL = schema

DataFrame = data

_____________________________________________________________________________________________________________________________________________
4. Code Differences (CSV vs JSON)
CSV Read

```python
spark.read \
    .option("header", True) \
    .schema(schema) \
    .csv("path")
```
JSON Read

```python
spark.read \
    .schema(schema) \
    .json("path")
```
Seed Difference

    CSV needs .option("header", True)
    JSON does NOT
    JSON supports nested structures
    CSV does not

_____________________________________________________________________________________________________________________________________________
5. Transformation Differences

CSV Example

Often requires:
type casting
timestamp parsing
trimming
null handling

JSON Example
Often requires:
flattening nested fields
exploding arrays
selecting nested keys
Seed Difference

CSV = flat cleanup

JSON = structural cleanup

_____________________________________________________________________________________________________________________________________________
6. Drop vs Select
One Word: Direction
Two Words: Keep vs Remove
Business Analogy:
Select = choose what to keep.
Drop = throw away what you don’t need.

Select
```python
df.select("col1", "col2")
Drop
```
```python
df.drop("url")
```
Seed Difference
Select = whitelist
Drop = blacklist
_____________________________________________________________________________________________________________________________________________

Transformation Differences

One Word: Cleanup
Two Words: Flat vs Structural

Business Analogy:
CSV cleanup = cleaning a table.
JSON cleanup = reorganizing a folder.

CSV
Type casting
Timestamp parsing
Null handling

JSON
Flattening nested fields
Exploding arrays
Selecting nested keys

Seed Difference
CSV cleanup is simple. JSON cleanup is structural.
_____________________________________________________________________________________________________________________________________________

```code
+---------------------+-------------------------+-------------------------+-----------------------------+
|       Topic         |          CSV            |          JSON           |       Seed Difference       |
+---------------------+-------------------------+-------------------------+-----------------------------+
| Structure           | Flat                    | Nested                  | Table vs Tree               |
| Schema              | Simple                  | Complex                 | JSON supports hierarchy     |
| Reader API          | .csv()                  | .json()                 | Different parsers           |
| Header              | Must specify            | Not needed              | CSV needs header option     |
| Schema Style        | StructType or DDL       | StructType or DDL       | Same options                |
| Transformations     | Type cleanup            | Structural cleanup      | JSON may need flattening    |
| Drop vs Select      | Both work               | Both work               | Direction of filtering      |
| DataFrame vs DDL    | Data vs Definition      | Data vs Definition      | Blueprint vs Building       |
+---------------------+-------------------------+-------------------------+-----------------------------+
```
