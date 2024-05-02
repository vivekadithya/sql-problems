# SQL DATA TYPES

### Convert the funding_total_usd and founded_at_clean columns in the tutorial.crunchbase_companies_clean_date table to strings (varchar format) using a different formatting function for each one.
```sql
SELECT
    CAST(funding_total_usd AS varchar)
    , founded_at_clean::varchar
FROM
    tutorial.crunchbase_companies_clean_date;
```