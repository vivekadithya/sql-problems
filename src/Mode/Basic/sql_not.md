# SQL NOT

### Write a query that returns all rows for songs that were on the charts in 2013 and do not contain the letter "a".
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND year = 2013
    AND song_name NOT ILIKE '%a%';
```