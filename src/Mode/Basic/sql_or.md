# SQL OR

### Write a query that returns all rows for top-10 songs that featured either Katy Perry or Bon Jovi.
```sql
SELECT
    *
FROM    
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND year_rank <= 10
    AND (group_name LIKE '%Katy Perry%' OR group_name LIKE '%Bon Jovi%');
```

### Write a query that returns all songs with titles that contain the word "California" in either the 1970s or 1990s.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND song_name ILIKE '%california%'
    AND (( year BETWEEN 1970 AND 1979) OR (year BETWEEN 1990 AND 1999));
```

### Write a query that lists all top-100 recordings that feature Dr. Dre before 2001 or after 2009.
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    1 = 1
    AND year_rank <= 100
    AND group_name ILIKE '%dr. dre%'
    AND ( year < 2001 OR year > 2009);
```