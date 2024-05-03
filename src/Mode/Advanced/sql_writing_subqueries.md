# Writing Subqueries in SQL
### Write a query that selects all Warrant Arrests from the tutorial.sf_crime_incidents_2014_01 dataset, then wrap it in an outer query that only displays unresolved incidents. 
```sql
SELECT
	sub.*
FROM
	(
	SELECT 
		* 
	FROM
		tutorial.sf_crime_incidents_2014_01
	WHERE
		descript = 'WARRANT ARREST'
	) sub
WHERE
	sub.resolution = 'NONE';
```

### What if you wanted to figure out how many incidents get reported on average each day of the week?
```sql
SELECT 
  EXTRACT(MONTH FROM sub.date::TIMESTAMP) AS "month"
  , sub.day_of_week
  , AVG(iss_per_day)
FROM
  (SELECT
  	day_of_week
  	, date
  	, COUNT(*) iss_per_day
  FROM
  	tutorial.sf_crime_incidents_2014_01
  GROUP BY
  	1, 2)sub
GROUP BY 1, 2
ORDER BY 1, 2;
```

### Write a query that displays the average number of monthly incidents for each category. Hint: use tutorial.sf_crime_incidents_cleandate to make your life a little easier. 
```sql
SELECT
	sub.category
	, AVG(sub.num_incidents)
FROM
	(SELECT
		EXTRACT(MONTH FROM date::timestamp) AS "month"
		, category
		, COUNT(*) num_incidents
	FROM
		tutorial.sf_crime_incidents_2014_01
	GROUP BY
		1, 2) sub
GROUP BY
	1
ORDER BY
	1;
```

###  Write a query that displays all rows from the three categories with the fewest incidents reported. 
```sql
SELECT
	x.*
FROM
	tutorial.sf_crime_incidents_2014_01 x JOIN (SELECT
  		category
  		, COUNT(*) num_incidents
		FROM
		  tutorial.sf_crime_incidents_2014_01
		GROUP BY
		  1
		ORDER BY
		  2
		LIMIT 3) sub
ON x.category = sub.category;
```

### Write a query that counts the number of companies founded and acquired by quarter starting in Q1 2012. Create the aggregations in two separate queries, then join them.
```sql
SELECT
	COALESCE(acq.acquired_quarter, fun.funded_quarter) "quarter"
	, acq.num_acq_comp
	, fun.num_fun_comp
FROM
	(SELECT
		acquired_quarter
		, COUNT(DISTINCT company_permalink) AS "num_acq_comp"
	FROM
		tutorial.crunchbase_acquisitions
	WHERE
		1 = 1
		AND acquired_quarter >= '2012-Q1'
	GROUP BY
		acquired_quarter) acq
	JOIN
	(SELECT
		funded_quarter
		, COUNT(DISTINCT company_permalink) AS "num_fun_comp"
	FROM
		tutorial.crunchbase_investments
	WHERE
		1 = 1
		AND funded_quarter >= '2012-Q1'
	GROUP BY
		funded_quarter) fun
	ON acq.acquired_quarter = fun.funded_quarter
ORDER BY
	quarter;
```
### Write a query that ranks investors from the combined dataset above by the total number of investments they have made.
```sql
SELECT
	sub.investor_name
	, COUNT(*)
FROM
	 ( SELECT *
          FROM tutorial.crunchbase_investments_part1

         UNION ALL

        SELECT *
          FROM tutorial.crunchbase_investments_part2
       ) sub
GROUP BY
	1
ORDER BY
	2 DESC;
```

### Write a query that does the same thing as in the previous problem, except only for companies that are still operating. Hint: operating status is in tutorial.crunchbase_companies.
```sql
SELECT
	sub.investor_name
	, COUNT(*)
FROM
	 ( SELECT *
          FROM tutorial.crunchbase_investments_part1

         UNION ALL

        SELECT *
          FROM tutorial.crunchbase_investments_part2
       ) sub
	   JOIN tutorial.crunchbase_companies comp
	   ON sub.company_permalink = comp.permalink AND comp.status = 'operating'
GROUP BY
	1
ORDER BY
	2 DESC;
```