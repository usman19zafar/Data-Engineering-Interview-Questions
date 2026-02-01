ADF cannot dynamically switch dataset types, so one Copy Activity for multiple file formats will keep failing.
Correct design is one pipeline + Switch on fileType + one Copy per format (CSV/JSON/Parquet).
That’s the only stable, production-grade solution.


Option A (recommended):

In Copy Activity → Source → turn Schema drift = ON

Sink (Parquet) → Allow schema drift = ON

Option B (strict):

Ensure all CSVs have identical columns + order + header

Then re-run the pipeline as-is
