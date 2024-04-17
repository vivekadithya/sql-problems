# Self Join
### How many stops are in the database.
```sql
SELECT
    COUNT(id)
FROM
    stops;
```

### Find the id value for the stop 'Craiglockhart'
```sql
SELECT
    id
FROM
    stops
WHERE
    1 = 1
    AND name = 'Craiglockhart';
```

### Give the id and the name for the stops on the '4' 'LRT' service.
```sql
SELECT
    stops.id
    , stops.name
FROM
    stops JOIN route
    ON stops.id = route.stop
WHERE
    1 = 1
    AND route.num = '4'
    AND route.company = 'LRT'; 
```

### The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these stops have a count of 2. Add a HAVING clause to restrict the output to these two routes.
```sql
SELECT 
    company
    , num
    , COUNT(*)
FROM 
    route 
WHERE 
    1 = 1
    AND stop=149 
    OR stop=53
GROUP BY 
    company
    , num
HAVING 
    COUNT(*) = 2;
```

### Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.
```sql
SELECT 
    a.company
    , a.num
    , a.stop
    , b.stop
FROM 
    route a JOIN route b 
    ON (
            a.company=b.company 
            AND a.num=b.num
        )
WHERE 
    1 = 1
    AND a.stop=53 
    AND b.stop=149
```

### The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'
```sql
SELECT
    a.company
    , a.num
    , sa.name
    , sb.name
FROM
    route a JOIN route b
    ON a.num = b.num
    JOIN stops sa
    ON sa.id = a.stop
    JOIN stops sb
    ON sb.id = b.stop
WHERE
    1 = 1
    AND sa.name = 'Craiglockhart'
    AND sb.name = 'London Road';
//
SELECT
    a.company
    , a.num
    , sa.name
    , sb.name
FROM
    route a JOIN route b
    ON a.num = b.num
    JOIN stops sa
    ON sa.id = a.stop
    JOIN stops sb
    ON sb.id = b.stop
WHERE
    1 = 1
    AND sa.name = 'Fairmilehead'
    AND sb.name = 'Tollcross';
```

### Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')
```sql
SELECT
    DISTINCT a.company
    , a.num
FROM
    route a JOIN route b
    ON a.num = b.num
    JOIN stops sa
    ON sa.id = a.stop
    JOIN stops sb
    ON sb.id= b.stop
WHERE
    1 = 1
    AND sa.id = 115
    AND sb.id = 137;
```

### Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'
```sql
SELECT
    DISTINCT a.company
    , a.num
FROM
    route a JOIN route b
    ON a.num = b.num
    JOIN stops sa
    ON sa.id = a.stop
    JOIN stops sb
    ON sb.id= b.stop
WHERE
    1 = 1
    AND sa.name = 'Craiglockhart'
    AND sb.name = 'Tollcross';
```

### Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.
```sql
SELECT
    DISTINCT sb.name
    , b.company
    , b.num
FROM
    route a JOIN route b
    ON a.num = b.num
    JOIN stops sa
    ON sa.id = a.stop
    JOIN stops sb
    ON sb.id = b.stop
WHERE
    1 = 1
    AND sa.name = 'Craiglockhart'
    AND b.company = 'LRT';
```

### Find the routes involving two buses that can go from Craiglockhart to Lochend.Show the bus no. and company for the first bus, the name of the stop for the transfer, and the bus no. and company for the second bus.
```sql
SELECT
    a.num
    , a.company
    , sb.name
    , c.num
    , c.company
FROM
    route a JOIN route b
    ON a.num = b.num
    JOIN route c
    ON c.num = b.num
    JOIN stops sa
    ON sa.id = a.stop
    JOIN stops sc
    ON sc.id = c.stop
    JOIN stops sb
    ON sb.id = b.stop
WHERE
    1 = 1
    AND sa.name = 'Craiglockhart'
    AND sc.name = 'Lochend';
```