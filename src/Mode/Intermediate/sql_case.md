# SQL CASE

### Write a query that includes a column that is flagged "yes" when a player is from California, and sort the results with those players first.
```sql
SELECT
    player_name
    , CASE
        WHEN state = 'CA' THEN 'yes'
        ELSE 'no' END AS "ca_flagged"
FROM
    benn.college_football_players
ORDER BY
    2 DESC;
```

### Write a query that includes players' names and a column that classifies them into four categories based on height. Keep in mind that the answer we provide is only one of many possible answers, since you could divide players' heights in many ways.
```sql
SELECT
    player_name
    , CASE 
        WHEN height BETWEEN 0 AND 60 THEN '0 - 60'
        WHEN height BETWEEN 61 AND 70 THEN '61 - 70'
        WHEN height BETWEEN 71 AND 80 THEN '71 - 80'
        ELSE '80 +' END AS "height_classification"
FROM
  benn.college_football_players;
```

### Write a query that selects all columns from benn.college_football_players and adds an additional column that displays the player's name if that player is a junior or senior.
```sql
SELECT
  *
  , CASE
      WHEN year IN ('JR', 'SR') THEN player_name
      ELSE NULL END AS "JR_SR"
FROM benn.college_football_players;
```

### Write a query that counts the number of 300lb+ players for each of the following regions: West Coast (CA, OR, WA), Texas, and Other (everywhere else).
```sql
SELECT
    CASE
        WHEN state IN ('CA', 'OR', 'WA') THEN 'West Coast'
        WHEN state IN ('TX') THEN 'Texas'
        ELSE 'Other' END AS "region"
    , COUNT(1)
FROM
    benn.college_football_players
WHERE
    weight > 300
GROUP BY
    region;
```

### Write a query that calculates the combined weight of all underclass players (FR/SO) in California as well as the combined weight of all upperclass players (JR/SR) in California.
```sql
SELECT
    CASE
        WHEN yr IN  ('FR', 'SO') THEN 'underclass'
        ELSE 'upperclass' END AS "class"
    , SUM(weight)
FROM
    benn.college_football_players
WHERE
    state = 'CA'
GROUP BY
    1;
```

### Write a query that displays the number of players in each state, with FR, SO, JR, and SR players in separate columns and another column for the total number of players. Order results such that states with the most players come first.
```sql
SELECT
    state
    , SUM(CASE WHEN year = 'FR' THEN 1 ELSE 0 END) AS "fr_count"
    , SUM(CASE WHEN year = 'SO' THEN 1 ELSE 0 END) AS "so_count"
    , SUM(CASE WHEN year = 'SR' THEN 1 ELSE 0 END) AS "sr_count"
    , SUM(CASE WHEN year = 'JR' THEN 1 ELSE 0 END) AS "jr_count"
    , COUNT(1) AS "total"
FROM 
    benn.college_football_players
GROUP BY 
    state
ORDER BY
    total DESC;
```

### Write a query that shows the number of players at schools with names that start with A through M, and the number at schools with names starting with N - Z.
```sql
SELECT
    full_school_name
    , CASE
        WHEN LEFT(player_name, 1) BETWEEN 'A' AND 'M' THEN 'A - M'
        ELSE 'N - Z' END AS "names_class"
    , SUM(2)
FROM
    benn.college_football_players
GROUP BY
  1, 2
ORDER BY
  1, 2;
```