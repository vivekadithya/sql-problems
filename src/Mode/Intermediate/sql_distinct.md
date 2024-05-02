# SQL DISTINCT

### Write a query that returns the unique values in the year column, in chronological order.
```sql
SELECT
    DISTINCT year
FROM
    tutorial.aapl_historical_stock_price
ORDER BY
    1;
```

### Write a query that counts the number of unique values in the month column for each year.
```sql
SELECT
    year
    , COUNT( DISTINCT month)
FROM
    tutorial.aapl_historical_stock_price
GROUP BY
    1;
```

### Write a query that separately counts the number of unique values in the month column and the number of unique values in the `year` column.
```sql
SELECT
    COUNT(DISTINCT year)
    , COUNT(DISTINCT month)
FROM
    tutorial.aapl_historical_stock_price;
```