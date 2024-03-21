# SELECT within SELECT Tutorial

### List each country name where the population is larger than that of 'Russia'.
```sql
SELECT
    name
FROM
    world
WHERE
    population > (
        SELECT
            population
        FROM
            world
        WHERE
            name = 'Russia'
    );
```

### Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
SELECT
    name
FROM
    world
WHERE
    continent = 'Europe'
    AND gdp/population > (
        SELECT
            gdp/population
        FROM
            world
        WHERE
            name = 'United Kingdom'
    );
```

### List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
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
            name IN ('Argentina', 'Australia')
    )
ORDER BY
    name;
```

### Which country has a population that is more than United Kingdom but less than Germany? Show the name and the population.
```sql
SELECT
    name
    , population
FROM
    world
WHERE 
    population > (
        SELECT
            population
        FROM
            world
        WHERE
            name = 'United Kingdom'
    ) AND population < (
        SELECT
            population
        FROM
            world
        WHERE
            name = 'Germany'
    );
```

### Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```sql
SELECT
    name
    , CONCAT(ROUND((population/(
        SELECT
            population
        FROM
            world
        WHERE
            name = 'Germany'))*100, 0),'%')
FROM
    world
WHERE
    continent = 'Europe';

```

### Which countries have a GDP greater than every country in Europe?
```sql
SELECT
    name
FROM
    world
WHERE
    gdp > ALL(
        SELECT
            gdp
        FROM
            world
        WHERE
            continent = 'Europe'
    );
```

### Find the largest country (by area) in each continent, show the continent, the name and the area
```sql
SELECT
    continent
    , name
    , area
FROM 
    world x
WHERE
    x.area >= ALL(
        SELECT 
            area
        FROM
            world y
        WHERE
            y.continent=x.continent
    );
```

### List each continent and the name of the country that comes first alphabetically.
```sql
SELECT
    continent
    , name
FROM
    world x
WHERE
    x.name <= ALL (
        SELECT
            name
        FROM
            world y
        WHERE
            x.continent=y.continent
    );
```

### Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.

```sql
SELECT
    name
    , continent
    , population
FROM
    world x
WHERE
    x.continent IN (
        SELECT
            DISTINCT continent
        FROM
            world y
        where
            25000000 >= ALL(
                SELECT
                    population
                FROM
                    world z
                WHERE
                    y.continent=z.continent
            )
    );
```
### Some countries have populations more than three times that of all of their neighbours (in the same continent). Give the countries and continents.
```sql
SELECT
    name
    , continent
FROM
    world x
WHERE
    population/3 >= ALL(
        SELECT
            population
        FROM
            world y
        WHERE
            1=1
            AND x.continent=y.continent
            AND x.name != y.name
    );
```