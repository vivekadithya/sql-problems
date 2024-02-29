# SQL Lesson 12: Order of execution of a Query

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