# SUM and COUNT
### Show the total population of the world.
```sql
SELECT
    SUM(population)
FROM
    world;
```
### List all the continents - just once each.
```sql
SELECT
    DISTINCT continent
FROM
    world;
```

### Give the total GDP of Africa
```sql
SELECT
    SUM(gdp)
FROM
    world
WHERE
    continent = 'Africa';
```

### How many countries have an area of at least 1000000
```sql
SELECT
    COUNT(name)
FROM
    world
WHERE
    area >= 1000000;
```

### What is the total population of ('Estonia', 'Latvia', 'Lithuania')
```sql
SELECT
    SUM(population)
FROM
    world
WHERE
    name IN ('Estonia', 'Latvia', 'Lithuania');
```

### For each continent show the continent and number of countries.
```sql
SELECT
    continent
    , COUNT(*) num_of_contries
FROM
    world
GROUP BY
    1;
```

### For each continent show the continent and number of countries with populations of at least 10 million.
```sql
SELECT
    continent
    , COUNT(name)
FROM
    world
WHERE
    population >= 10000000
GROUP BY
    1;
```

### List the continents that have a total population of at least 100 million.
```sql
SELECT
    DISTINCT continent
FROM
    world
GROUP BY
    1
HAVING
    SUM(population) >= 100000000;
```