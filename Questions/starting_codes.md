Starting code must be deterministic
Parameters

Imports

Widgets

Metadata

These are stable and required.

dbutils.fs.ls() is not deterministic — it depends on the current state of storage.

2. Starting code must be reusable in ADF / Jobs
Jobs do not need or use display().
Pipelines do not need or use ls().
