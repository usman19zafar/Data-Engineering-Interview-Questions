MDF vs MDM — short, sharp answer
Term	What it is	Category
MDF	Mapping Data Flow	Data transformation / processing
MDM	Master Data Management	Data governance / management discipline

They live in completely different layers of the data stack.
_____________________________________________________________________________________________________________________________________________
🧰 MDF — Mapping Data Flow

Purpose: Transform data
Question it answers: “How do I reshape this data?”

Clean, filter, join, aggregate

Schema mapping (source → target)

Technical, pipeline-level

Executes during ETL/ELT

Examples:

ADF Mapping Data Flow

Spark DataFrame transformations

SQL transformations

👉 Think: pipes + logic

_____________________________________________________________________________________________________________________________________________
🧠 MDM — Master Data Management

Purpose: Manage authoritative business entities
Question it answers: “Which data is the truth?”

Single source of truth for:

Customers

Products

Vendors

Locations

Deduplication & survivorship rules

Golden records

Governance, stewardship, ownership

Tools/examples:

Informatica MDM

SAP MDG

Profisee

Custom MDM on top of a warehouse/lakehouse

👉 Think: business truth + governance

_____________________________________________________________________________________________________________________________________________
🔬 Concrete example (same data, different problems)
Scenario: Customer data from 3 systems

MDF problem

“Normalize columns, clean nulls, join sources, calculate derived fields.”

Solution:

Mapping Data Flow

Spark

SQL

MDM problem

“Customer A in CRM = Customer B in Billing — which name, email, and address win?”

Solution:

Matching rules

Survivorship logic

Steward approval

Golden customer record

_____________________________________________________________________________________________________________________________________________
🧱 Architectural layer view
```code
Sources
   ↓
[MDF / Transform Layer]  ← technical
   ↓
Curated Data
   ↓
[MDM Layer]              ← business & governance
   ↓
Consumers (BI, Apps, ML)
```
They can coexist, but they do not replace each other.

🧠 Interview-grade one-liner (use this)

MDF handles how data is transformed; MDM handles which data is authoritative.
