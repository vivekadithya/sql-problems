# SQL Order By

### Write a query that returns all rows from 2012, ordered by song title from Z to A.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND year = 2012
ORDER BY
    song_name DESC;
```

### Write a query that returns all rows from 2010 ordered by rank, with artists ordered alphabetically for each song.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND year = 2010
ORDER BY
    rank
    , artist;
```

### Write a query that shows all rows for which T-Pain was a group member, ordered by rank on the charts, from lowest to highest rank (from 100 to 1).
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND group_name ILIKE '%t-pain%'
ORDER BY
    rank DESC;
```

### Write a query that returns songs that ranked between 10 and 20 (inclusive) in 1993, 2003, or 2013. Order the results by year and rank, and leave a comment on each line of the WHERE clause to indicate what that line does
```sql
/*
This is how you can write a
multi-line comment!
*/
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1 -- Placeholder condition
    AND rank BETWEEN 10 AND 20 -- filters titles that have a rank between 10 and 20
    AND year IN (1993, 2003, 2013) -- filters titles that were sung in 1993, 2003, 2013
ORDER BY
    year
    , rank;
```