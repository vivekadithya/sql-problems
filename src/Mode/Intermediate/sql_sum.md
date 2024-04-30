# SQL SUM

### Write a query to calculate the average opening price (hint: you will need to use both COUNT and SUM, as well as some simple arithmetic.).
```sql
SELECT
    SUM(open) / COUNT(open) "avg_open_price"
FROM
    tutorial.aapl_historical_stock_price;
```