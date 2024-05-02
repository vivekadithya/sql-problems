# SQL INNER JOIN
### Write a query that displays player names, school names and conferences for schools in the "FBS (Division I-A Teams)" division.
```sql
SELECT
  player.player_name
  , school.school_name
  , school.conference
FROM
  benn.college_football_players player JOIN benn.college_football_teams school
  ON player.school_name = school.school_name
WHERE
  1 = 1
  AND school.division = 'FBS (Division I-A Teams)';
```