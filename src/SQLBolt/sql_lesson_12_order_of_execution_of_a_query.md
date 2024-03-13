# SQL Lesson 12: Order of execution of a Query

> Each query begins with finding the data we need in the database, and then filtering that data down.
> 1. FROM and JOIN
> 2. WHERE
> 3. GROUP BY
> 4. HAVING
> 5. SELECT
> 6. DISTINCT
> 7. ORDER BY
> 8. LIMIT / OFFSET

### Find the number of movies each director has directed
```sql
SELECT
    director
    , COUNT(*) AS movies_directed
FROM
    movies
GROUP BY
    director;
```

### Find the total domestic and international sales that can be attributed to each director
```sql
SELECT
    mov.director
    , SUM(boxofc.domestic_sales + boxofc.international_sales) AS overall_sales
FROM
    movies AS mov INNER JOIN boxoffice AS boxofc
    ON mov.id = boxofc.movie_id
GROUP BY
    mov.director;
```