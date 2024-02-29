# Easy

### Query all columns for all American cities in the CITY table with populations larger than 100000. The CountryCode for America is USA.

```sql
SELECT
    *
FROM
    city
WHERE
    population>100000 
    AND countrycode='USA';
```

### Query the NAME field for all American cities in the CITY table with populations larger than 120000. The CountryCode for America is USA.

```sql
SELECT
    name
FROM
    city
WHERE
    population>120000
    AND countrycode='USA';
```

### Query all columns (attributes) for every row in the CITY table.

```sql
SELECT
    *
FROM
    city;

```

### Query all columns for a city in CITY with the ID 1661.
```sql
SELECT
    *
FROM
    city
WHERE
    ID=1661;
```

### Query all attributes of every Japanese city in the CITY table. The COUNTRYCODE for Japan is JPN.
```sql
SELECT
    *
FROM
    city
WHERE
    countrycode='JPN';
```

### Query the names of all the Japanese cities in the CITY table. The COUNTRYCODE for Japan is JPN.
```sql
SELECT
    name
FROM
    city
WHERE
    countrycode='JPN';
```

### Query a list of CITY and STATE from the STATION table.
```sql
SELECT
    city
    , state
FROM
    station;
```

### Query a list of CITY names from STATION for cities that have an even ID number. Print the results in any order, but exclude duplicates from the answer.
```sql
SELECT
    DISTINCT city
FROM
    station
WHERE
    ID % 2 = 0;
```

### Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table.

```sql
SELECT
    COUNT(city) - COUNT(DISTINCT city)
FROM
    station;
```

### Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.
