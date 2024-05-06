# SQL WINDOW FUNCITONS
### Write a query modification of the above example query that shows the duration of each ride as a percentage of the total time accrued by riders from each start_terminal
```sql
SELECT
    start_terminal
    , duration_seconds
    , duration_seconds/SUM(duration_seconds) OVER (PARTITION BY start_terminal) * 100 AS "pct_total_time"
FROM
    tutorial.dc_bikeshare_q1_2012
WHERE
    start_time < '2012-01-08'
ORDER BY
    1, 3 DESC;
```
### Write a query that shows a running total of the duration of bike rides (similar to the last example), but grouped by end_terminal, and with ride duration sorted in descending order.
```sql
SELECT
    end_terminal
    , duration_seconds
    , SUM(duration_seconds) OVER (PARTITION BY end_terminal ORDER BY duration_seconds DESC) AS "tot_running_duration"
FROM
    tutorial.dc_bikeshare_q1_2012
WHERE
    start_time < '2012-01-08';
```
### Write a query that shows the 5 longest rides from each starting terminal, ordered by terminal, and longest to shortest rides within each terminal. Limit to rides that occurred before Jan. 8, 2012.
```sql
SELECT
  sub.*
FROM
  (SELECT
      start_terminal
      , DENSE_RANK() OVER (PARTITION BY start_terminal ORDER BY duration_seconds DESC) AS "ride_rank"
  FROM
      tutorial.dc_bikeshare_q1_2012
  WHERE
      1 = 1
      AND start_time < '2012-01-08') sub
WHERE
  1 = 1
  AND sub.ride_rank <= 5;
```

### Write a query that shows only the duration of the trip and the percentile into which that duration falls (across the entire dataset—not partitioned by terminal).
```sql
SELECT
    duration_seconds
    , NTILE(100) OVER (ORDER BY duration_seconds) AS "duration_ptile"
FROM
    tutorial.dc_bikeshare_q1_2012;

-- Using Window Alias
-- SELECT
--     duration_seconds
--     , NTILE(100) OVER window_ntile AS "duration_ptile"
-- FROM
--     tutorial.dc_bikeshare_q1_2012
-- WINDOW window_ntile AS
--     (ORDER BY duration_seconds);
```