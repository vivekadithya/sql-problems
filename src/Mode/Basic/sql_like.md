# SQL Like

### Write a query that returns all rows for which Ludacris was a member of the group.
```sql
SELECT 
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    group_name LIKE 'Ludacris';
```

### Write a query that returns all rows for which the first artist listed in the group has a name that begins with "DJ".
```sql
SELECT
    *
FROM
    tutorial.billboard_top_100_year_end
WHERE
    group_name LIKE 'DJ%';
```

