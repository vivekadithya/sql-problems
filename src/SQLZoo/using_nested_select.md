# Using nested SELECT

### List each country in the same continent as 'Brazil'.
```sql
SELECT
    name
FROM
    world x
WHERE
    x.continent = (
        SELECT
            continent
        FROM
            world y
        WHERE
            name = 'Brazil'
    );
```

### List each country and its continent in the same continent as 'Brazil' or 'Mexico'.
```sql
SELECT
    name
    , continent
FROM
    world
WHERE
    continent IN (
        SELECT
            continent
        FROM
            world
        WHERE
            name IN ('Brazil', 'Mexico')
    );
```

### Show the population of China as a multiple of the population of the United Kingdom
```sql
SELECT
    population/(
        SELECT
            population
        FROM
            world
        WHERE
            name='United Kingdom'
        )
FROM
    world
WHERE
    name='China';
```

### Show each country that has a population greater than the population of ALL countries in Europe.
```sql
SELECT
    name
FROM
    world
WHERE
    population >= ALL(
        SELECT
            population
        FROM
            world
        WHERE
            continent='Europe'
    );
```