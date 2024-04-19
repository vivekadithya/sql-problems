# SQL And

### Write a query that surfaces all rows for top-10 hits for which Ludacris is part of the Group.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND year_rank <= 10
    AND group_name LIKE '%Ludacris%';
```

### Write a query that surfaces the top-ranked records in 1990, 2000, and 2010.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND year_rank = 1
    AND year IN (1990, 2000, 2010);
```

### Write a query that lists all songs from the 1960s with "love" in the title.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND year BETWEEN 1960 AND 1969
    AND song_name LIKE '%love%';
```