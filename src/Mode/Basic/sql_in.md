# SQL IN

### Write a query that shows all of the entries for Elvis and M.C. Hammer.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    artist IN ('Elvis Presley', 'M.C. Hammer', 'Hammer', 'Jan Hammer');
```