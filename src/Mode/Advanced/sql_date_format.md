# SQL DATE FORMAT

### Write a query that counts the number of companies acquired within 3 years, 5 years, and 10 years of being founded (in 3 separate columns). Include a column for total companies acquired as well. Group by category and limit to only rows with a founding date.
```sql
SELECT
  CASE
    WHEN comp.category_code IS NULL THEN 'Unknown'
    ELSE  comp.category_code END AS "category"
  , SUM(
    CASE WHEN acq.acquired_at::TIMESTAMP < comp.founded_at::TIMESTAMP + INTERVAL '3 years' THEN 1
    ELSE 0 END
  ) AS "3_years"
  , SUM(
    CASE WHEN acq.acquired_at::TIMESTAMP BETWEEN comp.founded_at::TIMESTAMP + INTERVAL '3 years' AND  comp.founded_at::TIMESTAMP + INTERVAL '5 years' THEN 1
    ELSE 0 END
  ) AS "5_years"
  , SUM(
    CASE WHEN acq.acquired_at::TIMESTAMP BETWEEN comp.founded_at::TIMESTAMP + INTERVAL '5 years' AND  comp.founded_at::TIMESTAMP + INTERVAL '10 years' THEN 1
    ELSE 0 END
  ) AS "10_years"
  , COUNT(acq.acquirer_name) AS "total_comp_acq"
FROM
  tutorial.crunchbase_companies comp JOIN tutorial.crunchbase_acquisitions acq
  ON comp.permalink = acq.company_permalink AND comp.founded_at IS NOT NULL
GROUP BY
  1
ORDER BY
  5 DESC;
```
