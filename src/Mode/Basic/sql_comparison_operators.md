# SQL Comparison Operators

### Did the West Region ever produce more than 50,000 housing units in one month?
```
SELECT
    *
FROM
    tutorial.us_housing_units
WHERE
    west > 50000;
```

### Did the South Region ever produce 20,000 or fewer housing units in one month?
```sql
SELECT
    *
FROM
    tutorial.us_housing_units
WHERE
    south <= 20000;
```

### Write a query that only shows rows for which the month name is February.
```sql
SELECT
    *
FROM
    tutorial.us_housing_units
WHERE
    month_name = 'February';
```

### Write a query that only shows rows for which the month_name starts with the letter "N" or an earlier letter in the alphabet.
```sql
SELECT
    *
FROM
    tutorial.us_housing_units
WHERE
    month_name <= 'N';
```

### Write a query that calculates the sum of all four regions in a separate column.
```sql
SELECT
    west + south  + midwest + northeast "all_sum"
FROM
    tutorial.us_housing_units;
```

### Write a query that returns all rows for which more units were produced in the West region than in the Midwest and Northeast combined.
```sql
SELECT
    *
FROM
    tutorial.us_housing_units
WHERE
    west > (midwest + northeast);
```

### Write a query that calculates the percentage of all houses completed in the United States represented by each region. Only return results from the year 2000 and later.
```sql
SELECT
    (west / (west + south  + midwest + northeast))*100 AS west_pct
    , (south / (west + south  + midwest + northeast))*100 AS south_pct
    , (midwest / (west + south  + midwest + northeast))*100 AS midwest_pct
    , (northeast / (west + south  + midwest + northeast))*100 AS northeast_pct
FROM
    tutorial.us_housing_units
WHERE
    year > 1999;
```