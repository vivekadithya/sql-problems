# More JOIN operations

### List the films where the yr is 1962 [Show id, title]
```sql
SELECT
    id
    , title
FROM
    movie
WHERE
    yr = 1962;
```

### When was Citizen Kane released?
```sql
SELECT
    yr
FROM
    movie
WHERE
    title = 'Citizen Kane';
```

### List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
```sql
SELECT
    id
    , title
    , yr
FROM
    movie
WHERE
    title LIKE '%Star Trek%'
ORDER BY
    yr;
```

### What id number does the actor 'Glenn Close' have?
```sql
SELECT
    id
FROM
    actor
WHERE
    name = 'Glenn Close';
```

### What is the id of the film 'Casablanca'
```sql
SELECT
    id
FROM
    movie
WHERE
    title = 'Casablanca';
```

### Obtain the cast list for 'Casablanca'.
```sql
SELECT
    actor.name
FROM
    actor JOIN casting
    ON actor.id = casting.actorid
WHERE
    casting.movieid = 11768;
```

### Obtain the cast list for the film 'Alien'
```sql
SELECT
    actor.name
FROM
    actor JOIN casting
    ON actor.id = casting.actorid
    JOIN movie
    ON casting.movieid = movie.id
WHERE
    movie.title = 'Alien';
```

### List the films in which 'Harrison Ford' has appeared
```sql
SELECT
    movie.title
FROM
    movie JOIN casting
    ON movie.id = casting.movieid
    JOIN actor
    ON actor.id = casting.actorid
WHERE
    actor.name = 'Harrison Ford';
```

### List the films where 'Harrison Ford' has appeared - but not in the starring role
```sql
SELECT
    movie.title
FROM
    movie JOIN casting
    ON movie.id = casting.movieid
    JOIN actor
    ON casting.actorid = actor.id
WHERE
    1 = 1
    AND actor.name = 'Harrison Ford'
    AND casting.ord != 1;
```

### List the films together with the leading star for all 1962 films.
```sql
SELECT
    movie.title
    , actor.name
FROM
    movie JOIN casting
    ON movie.id = casting.movieid
    JOIN actor
    ON casting.actorid = actor.id
WHERE
    1 = 1
    AND casting.ord = 1
    AND movie.yr = 1962;
```

### Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
```sql
SELECT
    movie.yr
    , COUNT(*)
FROM
    movie JOIN casting
    ON movie.id=casting.movieid
    JOIN actor
    ON actor.id=casting.actorid
WHERE
    1 = 1
    AND actor.name = 'Rock Hudson'
GROUP BY
    movie.yr
HAVING
    COUNT(*) > 2;
```

### List the film title and the leading actor for all of the films 'Julie Andrews' played in.
```sql
SELECT
    movie.title
    , actor.name
FROM
    movie JOIN casting
    ON movie.id = casting.movieid
    JOIN actor
    ON actor.id = casting.actorid
WHERE
    1 = 1
    AND movie.id IN (
        SELECT
            movie.id
        FROM
            movie JOIN casting
            ON movie.id = casting.movieid
            JOIN actor
            ON actor.id = casting.actorid
        WHERE
            actor.name = 'Julie Andrews' 
    ) AND casting.ord = 1;
```

### Obtain a list, in alphabetical order, of actors who've had at least 15 starring roles.
```sql
SELECT
    actor.name
FROM
    actor JOIN casting
    ON actor.id = casting.actorid
WHERE
    casting.ord = 1
GROUP BY
    actor.name
HAVING
    COUNT(*) >= 15;
```

### List the films released in the year 1978 ordered by the number of actors in the cast, then by title.
```sql
SELECT
    movie.title
    , COUNT(casting.actorid)
FROM
    movie JOIN casting
    ON movie.id = casting.movieid
WHERE
    movie.yr = 1978
GROUP BY
    movie.title
ORDER BY
    COUNT(casting.actorid) DESC
    , movie.title;
```

### List all the people who have worked with 'Art Garfunkel'.
```sql
SELECT
    DISTINCT actor.name
FROM
    actor JOIN casting
    ON actor.id = casting.actorid
WHERE
    1 = 1
    AND casting.movieid IN (
        SELECT
            casting.movieid
        FROM
            casting JOIN actor
            ON casting.actorid = actor.id
        WHERE
            actor.name = 'Art Garfunkel'
    )
    AND actor.name != 'Art Garfunkel';
```