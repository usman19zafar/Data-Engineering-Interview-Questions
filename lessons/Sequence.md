```code
INGRESS
  1. Connect to source
  2. Extract data
  3. Transport data
  4. Land in RAW

RAW
  5. Land raw files
  6. Store as-is
  7. Add ingestion metadata

BRONZE
  8. Validate file structure
  9. Basic cleanup (optional)
 10. Add ingestion lineage
 11. Write Bronze

SILVER
 12. Define metadata parameters
 13. Load schema contract
 14. Load mapping file
 15. Apply mapping rules
 16. Apply schema enforcement
 17. Apply dedupe (business key)
 18. Add metadata columns
 19. Write Silver

GOLD
 20. Apply business rules
 21. Join Silver tables
 22. Aggregate + calculate metrics
 23. Build facts/dimensions
 24. Write Gold

EGRESS
 25. Format for delivery
 26. Apply security/masking
 27. Deliver to external systems
 28. Log + monitor delivery
```
