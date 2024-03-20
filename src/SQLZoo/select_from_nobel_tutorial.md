# SELECT from Nobel Tutorial

### Change the query shown so that it displays Nobel prizes for 1950.
```sql
SELECT
    yr
    , subject
    , winner
FROM
    nobel
WHERE
    yr = 1950;
```
### Show who won the 1962 prize for literature.
```sql
SELECT
    winner
FROM
    nobel
WHERE
    yr = 1962 AND subject = 'Literature';
```

### Show the year and subject that won 'Albert Einstein' his prize.

```sql
SELECT
    yr
    , subject
FROM
    nobel
WHERE
    winner = "Albert Einstein";
```

### Give the name of the 'peace' winners since the year 2000, including 2000.
```sql
SELECT
    winner
FROM
    nobel
WHERE
    subject = 'Peace'
    AND yr >= 2000;
```

### Show all details (yr, subject, winner) of the literature prize winners for 1980 to 1989 inclusive.
```sql
SELECT
    yr
    , subject
    , winner
FROM
    nobel
WHERE
    subject = 'Literature'
    AND yr BETWEEN 1980 AND 1989;
```
### Show all details of the presidential winners:
```sql
SELECT
    *
FROM
    nobel
WHERE
    winner IN (
        'Theodore Roosevelt',
        'Thomas Woodrow Wilson',
        'Jimmy Carter',
        'Barack Obama'
    );
```

### Show the winners with first name John
```sql
SELECT
    winner
FROM
    nobel
WHERE
    winner LIKE 'John%';
```

### Show the year, subject, and name of physics winners for 1980 together with the chemistry winners for 1984.
```sql
SELECT
    yr
    , subject
    , winner
FROM
    nobel
WHERE
    (subject = 'Physics' AND yr = 1980) 
    OR (subject = 'Chemistry' AND yr = 1984);
```

### Show the year, subject, and name of winners for 1980 excluding chemistry and medicine
```sql
SELECT
    yr
    , subject
    , winner
FROM
    nobel
WHERE
    yr = 1980
    AND subject NOT IN (
        'Chemistry'
        , 'Medicine'
        );
```

### Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
```sql
SELECT
    yr
    , subject
    , winner
FROM
    nobel
WHERE
    (
        subject = 'Medicine'
        AND yr < 1910
    )
    OR (
        subject = 'Literature'
        AND yr >= 2004
    );
```

### Find all details of the prize won by PETER GRÜNBERG
```sql
SELECT
    *
FROM
    nobel
WHERE
    winner = 'PETER GRÜNBERG';
```

### Find all details of the prize won by EUGENE O'NEILL
```sql
SELECT
    *
FROM
    nobel
WHERE
    winner = 'EUGENE O''NEILL';
```

### List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```sql
SELECT
    winner
    , yr
    , subject
FROM
    nobel
WHERE
    winner LIKE 'Sir%'
ORDER BY
    yr DESC
    , winner;
```

### Show the 1984 winners and subject ordered by subject and winner name; but list chemistry and physics last.
```sql
SELECT
    temp.winner
    , temp.subject
FROM
    (
    SELECT
        winner
        , subject
        , CASE 
            WHEN subject NOT IN ('Physics', 'Chemistry') THEN 0 ELSE 1 END AS filt
    FROM
        nobel
    WHERE
        yr=1984) temp
ORDER BY
    temp.filt
    , temp.subject
    , temp.winner;
```