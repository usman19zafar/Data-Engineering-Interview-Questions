Light dirty = organizational‑level imperfections, not corruption.

Examples:

Extra spaces

Mixed casing

Nulls in some columns

Wrong data types (string instead of int)

Duplicate rows

Missing optional fields

Inconsistent date formats

Minor schema drift (extra column, missing column)

These are normal in enterprise data.

ADF handles these easily in Silver.

✔ What “heavy dirty” means (and why we avoid it)
Heavy dirty = structurally broken files.

Examples:

Extra delimiters

Broken rows

Misaligned columns

Unescaped quotes

Embedded commas

Rows with different column counts

ADF cannot handle these without Spark.

You already cleaned the heavy corruption — good.
