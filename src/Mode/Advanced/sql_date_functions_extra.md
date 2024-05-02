# PostgreSQL Date Functions and 7 Ways to Use Them in Business Analysis

Resource Link [https://mode.com/blog/postgres-sql-date-functions?utm_medium=referral&utm_source=mode-site&utm_campaign=sql-tutorial]

## Rounding off timestamps with DATE_TRUNC function
*SYNTAX:* DATE_TRUNC('interval', timestamp)

eg: DATE_TRUNC('day', '2015-04-12 14:44:18') => '2015-04-12 00:00:00'

###  How has web traffic changed over time?
```sql
SELECT
    DATE_TRUNC('day', occurred_at) AS "date_occurred"
    , COUNT(*) AS "Traffic"
FROM
    demo.web_events
GROUP BY
    date_occurred
ORDER BY
    date_occurred;
```

## Finding events relative to the present time with NOW() and CURRENT_DATE functions

*NOW() returns the whole timestamp, whereas the CURRENT_DATE() returns only the current date*

### What orders were placed in the last 8 years?
```sql
SELECT
    *
FROM
    demo.orders
WHERE
    1 = 1
    AND occurred_at::timestamp >= NOW() - INTERVAL '8 year';
```

### What orders were placed this day a 8 years ago?
```sql
SELECT
    *
FROM
    demo.orders
WHERE
    1 = 1
    -- AND DATE_TRUNC('day', occurred_at) = DATE_TRUNC('day', NOW() - INTERVAL '7 year');
    AND DATE_TRUNC('day', occurred_at) = CURRENT_DATE() - INTERVAL '7 year';
```

## Isolating hour-of-day and day-of-week with EXTRACT
*SYNTAX:* EXTRACT(subfield FROM timestamp)
### How many orders are placed each hour of the day?
```sql
SELECT
    EXTRACT('hour' FROM occurred_at) AS "hour_of_day"
    , COUNT(*)
FROM
    demo.orders
GROUP BY
    hour_of_day
ORDER BY
    hour_of_day;
```

### What's the average weekday order volume?
```sql
SELECT AVG(avg_weekly_order_vol)
FROM (
    SELECT
        EXTRACT('dow' FROM occurred_at) AS "day_of_week"
        , DATE_TRUNC('day', occurred_at) AS "day"
        , COUNT(id) AS "avg_weekly_order_vol"
    FROM
        demo.orders
    GROUP BY
        day_of_week
        , day
) a
WHERE day_of_week NOT IN (0, 6);
;
```

## Calculating time elapsed with AGE
### How old is a customer account?
```sql
SELECT
    id
    , AGE(created)
FROM
    modeanalytics.customer_accounts;
```

### How long does it take users to complete their profile each month, on average?
```sql
SELECT
    DATE_TRUNC('month', started_at) AS month
    , EXTRACT( 'epoch' FROM AVG(AGE(started_at, ended_at)))
FROM
    modeanalytics.profile_creation_events
GROUP BY 1
ORDER BY 1;
```