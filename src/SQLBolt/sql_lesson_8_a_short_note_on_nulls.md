# SQL Lesson 8: A short note on NULLs

### Find the name and role of all employees who have not been assigned to a building
```sql
SELECT 
    emp.name
    , emp.role
FROM
    employees AS emp LEFT JOIN buildings AS bldg
    ON emp.building=bldg.building_name
WHERE
    emp.building IS NULL;
```

### Find the names of the buildings that hold no employees
```sql
SELECT 
    bldg.building_name
FROM
    buildings AS bldg LEFT JOIN employees AS emp
    ON bldg.building_name = emp.building
WHERE
    emp.name IS NULL;
```