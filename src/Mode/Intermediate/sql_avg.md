# SQL AVG

### Write a query that calculates the average daily trade volume for Apple stock.
```sql
SELECT
    AVG(volume) AS "avg_trade_vol"
FROM
    tutorial.aapl_historical_stock_price;
```