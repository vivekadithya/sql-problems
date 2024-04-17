# Using Null

### List the teachers who have NULL for their department.
```sql
SELECT
    name
FROM
    teacher
WHERE
    dept IS NULL;
```

### Use a different JOIN so that all teachers are listed.
```sql
SELECT
    teacher.name
    , dept.name
FROM
    teacher LEFT JOIN dept
    ON teacher.dept = dept.id;
```

### Use a different JOIN so that all departments are listed.
```sql
SELECT
    teacher.name
    , dept.name 
FROM
    teacher RIGHT JOIN dept
    ON teacher.dept = dept.id;
```

### Show teacher name and mobile number or '07986 444 2266'
```sql
SELECT
    teacher.name
    , COALESCE(teacher.mobile, '07986 444 2266') mobile
FROM
    teacher;
```

### Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where there is no department.
```sql
SELECT
    teacher.name
    , COALESCE(dept.name, 'None')
FROM
    teacher LEFT JOIN dept
    ON dept.id = teacher.dept;
```

### Use COUNT to show the number of teachers and the number of mobile phones.
```sql
SELECT
    COUNT(teacher.name)
    , COUNT(teacher.mobile)
FROM
    teacher;
```

### Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.
```sql
SELECT
    dept.name
    , COUNT(teacher.name)
FROM
    teacher RIGHT JOIN dept
    ON teacher.dept = dept.id
GROUP BY
    dept.name;
```

### Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2 and 'Art' otherwise.
```sql
SELECT 
    name
    , CASE 
        WHEN dept IN (1, 2) THEN 'Sci'
        ELSE 'Art'
    END AS dept_name
FROM
    teacher;
```

### Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise.
```sql
SELECT 
    name
    , CASE 
        WHEN dept IN (1, 2) THEN 'Sci'
        WHEN dept = 3 THEN 'Art'
        ELSE 'None'
    END AS dept_name
FROM
    teacher;
```