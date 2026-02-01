ADF cannot dynamically switch dataset types, so one Copy Activity for multiple file formats will keep failing.
Correct design is one pipeline + Switch on fileType + one Copy per format (CSV/JSON/Parquet).
That’s the only stable, production-grade solution.
