JSON STRING = text → useless until parsed

PARSING = convert text → struct

STRUCT = real nested columns Spark can query

NESTED JSON STRING = JSON inside JSON → parse inner layer too


1. JSON STRING
Type: string  
Meaning: Raw text that looks like JSON but Spark treats it as plain text.

Example value:

Code
{"id":123,"name":"Ali","city":"Milton"}
Spark sees this as:

Code
"{\"id\":123,\"name\":\"Ali\",\"city\":\"Milton\"}"
Problem:  
You cannot access fields.
You cannot do df.customer_info.id.

Action:  
You must parse it.

2. PARSING
Meaning: Convert a JSON STRING → STRUCT using a schema.

Code:

python
df = df.withColumn(
    "customer_info",
    F.from_json("customer_info", customer_schema)
)
Result:  
Spark creates real columns inside the struct.

3. STRUCT
Type: struct  
Meaning: A nested object with real typed fields Spark can use.

Example schema:

Code
|-- customer_info: struct
|   |-- id: long
|   |-- name: string
|   |-- city: string
You can now do:

python
df.select("customer_info.id")
No parsing needed — it’s already structured.

4. NESTED JSON STRING
Meaning: A JSON STRING inside another JSON object.

Example:

Code
{
  "customer_id": 1,
  "details": "{\"age\":30,\"gender\":\"M\"}"
}
Schema:

Code
|-- customer_id: long
|-- details: string   ← JSON string inside
Action:  
You must parse the inner string too:

python
df = df.withColumn(
    "details",
    F.from_json("details", details_schema)
)
After parsing, details becomes a struct.
