# SQL COUNT

### Write a query to count the number of non-null rows in the low column.
```sql
SELECT
    COUNT(low) AS "non_null_low"
FROM
    tutorial.aapl_historical_stock_price
WHERE
    low IS NOT NULL;
```

### Write a query that determines counts of every single column. With these counts, can you tell which column has the most null values?
```sql
SELECT
    COUNT(date) AS "date_count"
    , COUNT(year) AS "year_count"
    , COUNT(month) AS "month_count"
    , COUNT(open) AS "open_count"
    , COUNT(high) AS "high_count" -- column with the most null values
    , COUNT(low) AS "low_count"
    , COUNT(close) AS "close_count"
    , COUNT(volume) AS "volume_count"
    , COUNT(id) AS "id_count"
FROM
    tutorial.aapl_historical_stock_price;
```