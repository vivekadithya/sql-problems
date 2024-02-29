# SQL Lesson 7: OUTER JOINs

### Find the list of all buildings that have employees
```sql
SELECT 
    DISTINCT bldg.building_name 
FROM 
    employees AS emp LEFT JOIN buildings AS bldg
    ON emp.building=bldg.building_name;
```

### Find the list of all buildings and their capacity
```sql
SELECT 
    *
FROM 
    buildings AS bldg;
```

### List all buildings and the distinct employee roles in each building (including empty buildings)
```sql
SELECT 
    DISTINCT bldg.building_name
    , emp.role
FROM 
    buildings AS bldg LEFT JOIN employees AS emp
    ON bldg.building_name=emp.building;
```