# SQL Joins Using WHERE or ON

### Write a query that shows a company's name, "status" (found in the Companies table), and the number of unique investors in that company. Order by the number of investors from most to fewest. Limit to only companies in the state of New York.
```sql
SELECT
    comp.name
    , comp.status
    , COUNT(DISTINCT inv.investor_name)
FROM
    tutorial.crunchbase_companies comp LEFT JOIN tutorial.crunchbase_investments inv
    ON comp.permalink = inv.company_permalink
    AND comp.state_code = 'NY'
GROUP BY
    1, 2
ORDER BY
    3 DESC;
```

### Write a query that lists investors based on the number of companies in which they are invested. Include a row for companies with no investor, and order from most companies to least.
```sql
SELECT
    CASE 
        WHEN inv.investor_name IS NULL THEN 'No investor'
        ELSE inv.investor_name END as investor
    , COUNT(DISTINCT inv.company_permalink)
FROM
    tutorial.crunchbase_companies comp LEFT JOIN tutorial.crunchbase_investments inv
    ON comp.permalink = inv.company_permalink
GROUP BY
    1
ORDER BY 
    2 DESC;
```