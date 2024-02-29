# SQL Lesson 6: Multi-table queries with JOINs

### Find the domestic and international sales for each movie
```sql
SELECT 
    movie.title
    , boxofc.domestic_sales
    , boxofc.international_sales
FROM 
    movies AS "movie" INNER JOIN boxoffice AS "boxofc"
    ON movie.id=boxofc.movie_id;
```

### Show the sales numbers for each movie that did better internationally rather than domestically
```sql
SELECT 
    movie.title
    , boxofc.domestic_sales
    , boxofc.international_sales
FROM 
    movies AS "movie" INNER JOIN boxoffice AS "boxofc"
    ON movie.id=boxofc.movie_id
WHERE
    boxofc.domestic_sales < boxofc.international_sales;
```

### List all the movies by their ratings in descending order
```sql
SELECT 
    movie.title
FROM 
    movies AS "movie" INNER JOIN boxoffice AS "boxofc"
    ON movie.id=boxofc.movie_id
ORDER BY
    rating DESC;
```
