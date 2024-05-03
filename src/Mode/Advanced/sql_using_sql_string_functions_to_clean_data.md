# Using SQL String Functions to Clean Data

### Write a query that separates the `location` field into separate fields for latitude and longitude. You can compare your results against the actual `lat` and `lon` fields in the table.
```sql
SELECT
    location
    , lat
    , TRIM( leading '(' FROM LEFT(location, POSITION(',' IN location)-1)) AS "clean_lat"
    , TRIM( trailing ')' FROM SUBSTR(location, POSITION('-' IN location), POSITION(')' IN location)-1)) AS "clean_lon"
FROM
    tutorial.sf_crime_incidents_2014_01;
```

### Concatenate the lat and lon fields to form a field that is equivalent to the location field. (Note that the answer will have a different decimal precision.)
```sql
SELECT
    CONCAT('(', lat, ',', lon, ')') AS "concat_lat_loin"
FROM
    tutorial.sf_crime_incidents_2014_01;
```

### Create the same concatenated location field, but using the || syntax instead of CONCAT.
```sql
SELECT
    '(' || lat || ',' || lon || ')' AS "piped_concat_lat_lon"
FROM
    tutorial.sf_crime_incidents_2014_01;
```

### Write a query that creates a date column formatted YYYY-MM-DD.
```sql
SELECT
    EXTRACT('year' FROM date::timestamp) || '-' || LEFT(date, 2) || '-' || SUBSTR(date, 4, 2) AS "formatted_date"
FROM
    tutorial.sf_crime_incidents_2014_01;
```

### Write a query that returns the `category` field, but with the first letter capitalized and the rest of the letters in lower-case.
```sql
SELECT
    UPPER(LEFT(category, 1)) || LOWER(SUBSTR(category, 2, LENGTH(category)-1))
FROM
    tutorial.sf_crime_incidents_2014_01;
```

### Write a query that creates an accurate timestamp using the date and time columns in tutorial.sf_crime_incidents_2014_01. Include a field that is exactly 1 week later as well.
```sql
SELECT
   (LEFT(date, 10) || ' ' || time)::TIMESTAMP AS "timestamp"
   , (LEFT(date, 10) || ' ' || time)::TIMESTAMP + INTERVAL '1 week' AS "timestamp + 1wk"
FROM
    tutorial.sf_crime_incidents_2014_01;
```

### Write a query that counts the number of incidents reported by week. Cast the week as a date to get rid of the hours/minutes/seconds.
```sql
SELECT
    DATE_TRUNC('week', date::TIMESTAMP) AS "week_date"
    , COUNT(*)
FROM
    tutorial.sf_crime_incidents_2014_01
GROUP BY
    week_date
ORDER BY
    week_date;
```

### Write a query that shows exactly how long ago each indicent was reported. Assume that the dataset is in Pacific Standard Time (UTC - 8).
```sql
SELECT
    *
    , CURRENT_TIMESTAMP AT TIME ZONE 'PST' - data::TIMESTAMP AS "time_since_incident"
FROM
    tutorial.sf_crime_incidents_2014_01;
```