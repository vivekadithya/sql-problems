# SQL Lesson 9: Queries with expressions

### List all movies and their combined sales in millions of dollars
```sql
SELECT 
    mov.title
    , (boxofc.domestic_sales + boxofc.international_sales)/1000000 AS combined_sales
FROM 
    movies AS mov INNER JOIN boxoffice AS boxofc
    ON mov.id=boxofc.movie_id;
```

### List all movies and their ratings in percent
```sql
SELECT 
    mov.title
    , rating*10 AS rating_pct
FROM 
    movies AS mov INNER JOIN boxoffice AS boxofc
    ON mov.id=boxofc.movie_id;
```

### List all movies that were released on even number years
```sql
SELECT 
    mov.title
FROM 
    movies AS mov INNER JOIN boxoffice AS boxofc
    ON mov.id=boxofc.movie_id
WHERE
    mov.year%2=0;
```