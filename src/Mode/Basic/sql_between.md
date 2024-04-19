# SQL Between

### Write a query that shows all top 100 songs from January 1, 1985 through December 31, 1990.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1=1
    AND year_rank = 1
    AND year BETWEEN '01-01-1985' AND '12-31-1990';
```