# SQL MIN / MAX

### What was Apple's lowest stock price (at the time of this data collection)?
```sql
SELECT
    MIN(low)
FROM
    tutorial.aapl_historical_stock_price;
```

### What was the highest single-day increase in Apple's share value?
```sql
SELECT
    MAX(high)
FROM
    tutorial.aapl_historical_stock_price;
```