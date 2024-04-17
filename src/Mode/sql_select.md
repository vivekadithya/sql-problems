# SQL SELECT

### Write a query to select all of the columns in the tutorial.us_housing_units table without using *.
```sql
SELECT
    *
FROM
    tutorial.us_housing_units;
```

### Write a query to select all of the columns in tutorial.us_housing_units and rename them so that their first letters are capitalized.
```sql
SELECT
    year AS "Year"
    , month AS "Month"
    , month_name AS "Month_name"
    , west AS "West"
    , midwest AS "Midwest"
    , south AS "South"
    , northeast AS "Northeast"
FROM
    tutorial.us_housing_units;
```