# SQL Lesson 11: Queries with aggregates (Pt. 2)

### Find the number of Artists in the studio (without a HAVING clause)
```sql
SELECT 
    COUNT(role) 
FROM 
    employees
WHERE
    role = 'artist';
```

### Find the number of Employees of each role in the studio
```sql
SELECT 
    role
    , COUNT(*)
FROM 
    employees
GROUP BY
    role;
```

### Find the total number of years employed by all Engineers
```sql
SELECT 
    SUM(years_employed)
FROM 
    employees
HAVING
    role='Engineer';
```