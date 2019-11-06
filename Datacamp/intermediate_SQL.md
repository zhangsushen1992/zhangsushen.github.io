## Intermediate SQL
The CASE statement acts as if statement:
```
SELECT
id,
home_goal,
away_goal,
CASE WHEN home_goal > away_goal THEN 'Home Team Win'
     WHEN home_goal < away_goal THEN 'Away Team Win'
     ELSE 'Tie' END AS outcome
FROM match
WHERE season = '2013/2014';
```
