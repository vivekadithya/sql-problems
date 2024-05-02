# SQL UNION

### Write a query that appends the two crunchbase_investments datasets above (including duplicate values). Filter the first dataset to only companies with names that start with the letter "T", and filter the second to companies with names starting with "M" (both not case-sensitive). Only include the company_permalink, company_name, and investor_name columns.
```sql
SELECT 
    company_permalink
    , company_name
    , investor_name
FROM tutorial.crunchbase_investments_part1
WHERE
    company_name ILIKE 'T%'

UNION ALL

SELECT
    company_permalink
    , company_name
    , investor_name
FROM tutorial.crunchbase_investments_part2
WHERE
    company_name ILIKE 'M%'
```

### Write a query that shows 3 columns. The first indicates which dataset (part 1 or 2) the data comes from, the second shows company status, and the third is a count of the number of investors. Hint: you will have to use the tutorial.crunchbase_companies table as well as the investments tables. And you'll want to group by status and dataset.
```sql
SELECT 
    'part 1' AS "dataset"
    , comp.status
    , COUNT(DISTINCT inv.investor_name) "investor_count"
FROM tutorial.crunchbase_companies comp LEFT JOIN tutorial.crunchbase_investments_part1 inv
  ON comp.permalink = inv.company_permalink
GROUP BY
  1, 2

UNION ALL

SELECT
    'part 2' AS "dataset"
    , comp.status
    , COUNT(DISTINCT inv.investor_name) "investor_count"
FROM tutorial.crunchbase_companies comp LEFT JOIN tutorial.crunchbase_investments_part2 inv
  ON comp.permalink = inv.company_permalink
GROUP BY
  1, 2;
```