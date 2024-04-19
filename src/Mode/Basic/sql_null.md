# SQL Null

### Write a query that shows all of the rows for which song_name is null.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND song_name IS NULL;
```
