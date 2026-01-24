```code
+----------------------+---------------------------------------------------------------+
| Component            | Value / Rule                                                  |
+----------------------+---------------------------------------------------------------+
| Input Format         | https://A.blob.core.windows.net/C/P                           |
| Output Format        | abfss://C@A.dfs.core.windows.net/P                            |
+----------------------+---------------------------------------------------------------+
| A (Account Name)     | Extract from URL before `.blob.core.windows.net`              |
| C (Container Name)   | First path segment after domain                               |
| P (Path + File)      | Everything after container                                    |
+----------------------+---------------------------------------------------------------+
| Example Input        | https://uzi.blob.core.windows.net/raw/constructors.json       |
| Example Output       | abfss://raw@uzi.dfs.core.windows.net/constructors.json        |
+----------------------+---------------------------------------------------------------+
| Conversion Formula   | abfss://<container>@<account>.dfs.core.windows.net/<path>     |
+----------------------+---------------------------------------------------------------+
```
