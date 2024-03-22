# The JOIN Operation
### Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
```sql
SELECT
    matchid
    , player
FROM
    goal
WHERE
    teamid = 'GER';
```

### Show id, stadium, team1, team2 for just game 1012
```sql
SELECT
    id
    , stadium
    , team1
    , team2
FROM
    game
WHERE
    id = 1012;
```

### Modify it to show the player, teamid, stadium and mdate for every German goal.
```sql
SELECT
    goal.player
    , goal.teamid
    , game.stadium
    , game.mdate
FROM
    game JOIN goal
    ON game.id=goal.matchid
WHERE
    goal.teamid='GER'
```

### Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
```sql
SELECT
    game.team1
    , game.team2
    , goal.player
FROM
    game JOIN goal
    ON game.id = goal.matchid
WHERE
    goal.player LIKE 'Mario%';
```

### Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
```sql
SELECT
    goal.player
    , goal.teamid
    , eteam.coach
    , goal.gtime
FROM
    eteam JOIN goal
    ON eteam.id = goal.teamid
WHERE
    goal.gtime <=10;
```

### List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
```sql
SELECT
    game.mdate
    , eteam.teamname
FROM
    game JOIN eteam
    ON game.team1 = eteam.id
WHERE
    eteam.coach='Fernando Santos';
```

### List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
```sql
SELECT
    goal.player
FROM
    goal JOIN game
    ON game.id = goal.matchid
WHERE
    game.stadium = 'National Stadium, Warsaw';
```

### Show the name of all players who scored a goal against Germany.
```sql
SELECT
    DISTINCT goal.player
FROM
    goal JOIN game
    ON goal.matchid = game.id
WHERE
    goal.teamid != 'GER'
    AND((game.team1='GER' AND game.team2!='GER')
    OR (game.team1 != 'GER' AND game.team2 = 'GER'));
```

### Show teamname and the total number of goals scored.
```sql
SELECT
    eteam.teamname
    , COUNT(*)
FROM
    eteam JOIN goal
    ON eteam.id = goal.teamid
GROUP BY
    eteam.teamname;
```

### Show the stadium and the number of goals scored in each stadium.
```sql
SELECT
    game.stadium
    , COUNT(*)
FROM
    game JOIN goal
    ON game.id = goal.matchid
GROUP BY
    game.stadium
```

### For every match involving 'POL', show the matchid, date and the number of goals scored.
```sql
SELECT
    game.id
    , game.mdate
    , COUNT(*)
FROM
    game JOIN goal
    ON game.id = goal.matchid
WHERE
    game.team1='POL' OR game.team2='POL'
GROUP BY
    1, 2;
```

### For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
```sql
SELECT
    game.id
    , game.mdate
    , COUNT(*)
FROM
    game JOIN goal
    ON  game.id = goal.matchid
WHERE
    (game.team1 = 'GER' OR game.team2 = 'GER')
    AND goal.teamid = 'GER'
GROUP BY
    1, 2;
```

### List every match with the goals scored by each team as shown. Sort your result by mdate, matchid, team1 and team2.
```sql
SELECT
    game.mdate
    , game.team1
    , SUM( CASE WHEN goal.teamid = game.team1 THEN 1 ELSE 0 END) AS score1
    , game.team2
    , SUM( CASE WHEN goal.teamid = game.team2 THEN 1 ELSE 0 END) AS score2
FROM
    game LEFT JOIN goal
    ON game.id = goal.matchid
GROUP BY
    game.mdate
    , game.id
    , game.team1
    , game.team2;
```