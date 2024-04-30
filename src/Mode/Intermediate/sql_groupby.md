# SQL GROUP BY

### Calculate the total number of shares traded each month. Order your results chronologically.
```sql
SELECT
    year
    , month
    , SUM(volume)
FROM
    tutorial.aapl_historical_stock_price
GROUP BY
    year, month
ORDER BY
    year, month;
```

### Write a query to calculate the average daily price change in Apple stock, grouped by year.
```sql
SELECT
    year
    , AVG(high - low)
FROM
    tutorial.aapl_historical_stock_price
GROUP BY
    year;
```

### Write a query that calculates the lowest and highest prices that Apple stock achieved each month.
```sql
SELECT
    year
    , month
    , MAX(high) "max_price"
    , MIN(low) "min_price"
FROM
    tutorial.aapl_historical_stock_price
GROUP BY
    year
    , month
```