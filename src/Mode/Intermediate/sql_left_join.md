# SQL LEFT JOIN

### Write a query that performs an inner join between the tutorial.crunchbase_acquisitions table and the tutorial.crunchbase_companies table, but instead of listing individual rows, count the number of non-null rows in each table.
```sql
SELECT
    COUNT(acq.company_permalink) "acq_count"
    , COUNT(comp.permalink) "comp_count"
FROM
    tutorial.crunchbase_acquisitions acq JOIN tutorial.crunchbase_companies comp
    ON acq.company_permalink = comp.permalink;
```

### Modify the query above to be a LEFT JOIN. Note the difference in results.
```sql
SELECT
    COUNT(acq.company_permalink) "acq_count"
    , COUNT(comp.permalink) "comp_count"
FROM
    tutorial.crunchbase_acquisitions acq LEFT JOIN tutorial.crunchbase_companies comp
    ON acq.company_permalink = comp.permalink;
```

### Count the number of unique companies (don't double-count companies) and unique acquired companies by state. Do not include results for which there is no state data, and order by the number of acquired companies from highest to lowest.
```sql
SELECT
    comp.state_code
    , COUNT(DISTINCT comp.permalink) "unique_companies"
    , COUNT(DISTINCT acq.company_permalink) "unique_acq_companies"
FROM
    tutorial.crunchbase_companies comp LEFT JOIN tutorial.crunchbase_acquisitions acq
    ON acq.company_permalink = comp.permalink
WHERE
    1 = 1
    AND comp.state_code IS NOT NULL
GROUP BY
    comp.state_code
ORDER BY
    unique_companies DESC;
```