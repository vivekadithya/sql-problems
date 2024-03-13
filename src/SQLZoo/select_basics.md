# Select Basics

### 1. Modify it to show the population of Germany
```sql
SELECT
    population
FROM
    world
WHERE
    name = 'Germany';
```

### 2. Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.
```sql
SELECT
    name
    , population
FROM
    world
WHERE
    name IN ('Sweden', 'Norway', 'Denmark');
```

### Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
```sql
SELECT
    name
    , area
FROM
    world
WHERE
    area BETWEEN 200000 AND 250000;
```