1) NDJSON (Newline‑Delimited JSON)
Most common in real pipelines.

```Code
{"id":1}
{"id":2}
{"id":3}
```
RAW Characteristics
One JSON object per line

No array brackets

No indentation

Streaming‑friendly

Log‑friendly

Bronze Strategy
```Code
df = spark.read.json("path")   # default mode
```
Rules
NEVER use multiLine=True

ALWAYS expect 1 row per line

ALWAYS validate with spark.read.text()

____________________________________________________________________________________________________________________________________________
2) JSON Array (Single Document Containing Many Objects)
```Code
[
  {"id":1},
  {"id":2},
  {"id":3}
]
```
RAW Characteristics
Starts with [

Ends with ]

Multi‑line

Entire dataset is one JSON document

Bronze Strategy
```Code
df = spark.read.option("multiLine", True).json("path")
```
Rules
multiLine=True is mandatory

Spark will produce correct rows automatically

____________________________________________________________________________________________________________________________________________
3) Pretty JSON (Single Object, Multi‑Line)
```Code
{
  "id": 1,
  "name": "Usman",
  "role": "Architect"
}
```
RAW Characteristics
One object

Multi‑line

Indented

Bronze Strategy
```Code
df = spark.read.option("multiLine", True).json("path")
```
Rules
Produces 1 row only

Useful for config files, metadata, settings

____________________________________________________________________________________________________________________________________________
4) Mixed JSON (Array + Objects + NDJSON)
Rare but real in messy systems.

Example:

```Code
{"id":1}
[
  {"id":2},
  {"id":3}
]
{"id":4}
```
RAW Characteristics
Invalid for Spark

Mixed formats

Often caused by bad upstream systems

Bronze Strategy
You must normalize RAW first:

```Code
raw = spark.read.text("path")
# filter lines starting with { and ending with }
clean = raw.filter(col("value").startswith("{"))
df = spark.read.json(clean.rdd.map(lambda r: r.value))
```
Rules
Treat as NDJSON after cleaning

Never use multiLine
____________________________________________________________________________________________________________________________________________
5) JSON with Comments (Non‑Standard)
Code
{
  // this is a comment
  "id": 1
}
RAW Characteristics
Not valid JSON

Spark will fail

Bronze Strategy
Strip comments first:

```Code
raw = spark.read.text("path")
clean = raw.filter(~col("value").startswith("//"))
df = spark.read.option("multiLine", True).json(clean.rdd.map(lambda r: r.value))
```

____________________________________________________________________________________________________________________________________________
6) JSON with Trailing Commas (Non‑Standard)

```Code
{
  "id": 1,
  "name": "Usman",
}
```
RAW Characteristics
Invalid JSON

Spark will fail

Bronze Strategy
Pre‑clean:

```Code
raw = spark.read.text("path")
clean = raw.withColumn("value", regexp_replace("value", ",\\s*}", "}"))
df = spark.read.option("multiLine", True).json(clean.rdd.map(lambda r: r.value))
```

____________________________________________________________________________________________________________________________________________
7) JSON Lines with Blank Lines
```Code
{"id":1}

{"id":2}

{"id":3}
```
RAW Characteristics
NDJSON with empty lines

Spark will error on empty lines

Bronze Strategy
```Code
raw = spark.read.text("path")
clean = raw.filter(length("value") > 0)
df = spark.read.json(clean.rdd.map(lambda r: r.value))
```

____________________________________________________________________________________________________________________________________________
8) JSON with Embedded Arrays

```Code
{
  "team": [
    {"id":1},
    {"id":2}
  ]
}
```
RAW Characteristics
One row

Nested array

Bronze Strategy

```Code
df = spark.read.option("multiLine", True).json("path")
df = df.withColumn("team", explode("team"))
```

____________________________________________________________________________________________________________________________________________
9) JSON with Deeply Nested Structures
```Code
{
  "race": {
    "season": 2024,
    "round": 1,
    "location": {
      "country": "Bahrain",
      "city": "Sakhir"
    }
  }
}
```
RAW Characteristics
Multi‑line

Deep nesting

Bronze Strategy

```Code
df = spark.read.option("multiLine", True).json("path")
df = df.select("race.*", "race.location.*")
```

____________________________________________________________________________________________________________________________________________
10) Corrupted JSON (Partial Objects)
```Code
{"id":1, "name":"Usman"
{"id":2}
```
RAW Characteristics
Broken braces

Missing commas

Invalid JSON

Bronze Strategy
You cannot ingest directly.  
You must fix RAW manually or with regex.

THE COMPLETE DECISION TREE (Your Real Learning)
```Code
IF file has one JSON object per line → NDJSON → multiLine=False
IF file starts with [ → JSON Array → multiLine=True
IF file spans multiple lines but no [ → Pretty JSON → multiLine=True
IF file has comments → strip → then multiLine=True
IF file has trailing commas → clean → then multiLine=True
IF file has blank lines → filter → then NDJSON
IF file is mixed → normalize → then NDJSON
IF file is nested → multiLine=True → flatten
IF file is corrupted → fix RAW first
This is the entire universe of JSON ingestion patterns for RAW → Bronze.
```
