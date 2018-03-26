# Joins - Football
This tutorial introduces JOIN which allows you to use data from two or more tables. The tables contain all matches and goals from UEFA EURO 2012 Football Championship in Poland and Ukraine.

## Tables
game

| id | mdate | stadium | team1 | team2 |
| --- |---| --- |---| --- |
| 1001 | 8  June 2012 | National Stadium, Warsaw  | POL | GRE |
| 1002 | 8  June 2012 | Stadion Miejski (Wroclaw) | RUS | CZE |
| 1003 | 12 June 2012 | Stadion Miejski (Wroclaw) | GRE | CZE |
| 1004 | 12 June 2012 | National Stadium, Warsaw  | POL | RUS |

goal

| matchid |teamid | player | gtime |
| --- |---| --- |---| 
| 1001 | POL | Robert Lewandowski | 17 |
| 1001 | GRE | Dimitris Salpingidis | 51 |
| 1002 | RUS | Alan Dzagoev | 15 |
| 1002 | RUS | Roman Pavlyuchenko | 82 |

eteam

| id |  teamname | coach |
| --- |---| --- |
| POL | Poland | Franciszek Smuda |
| RUS | Russia | Dick Advocaat |
| CZE | Czech Republic | Michal Bilek |
| GRE | Greece | Fernando Santos |

### 1. The first example shows the goal scored by a player with the last name 'Bender'. Modify it to show the matchid and player name for all goals scored by Germany. 
```
SELECT matchid, player FROM goal 
  WHERE teamid = 'ger'
```
### 2.
From the previous query you can see that Lars Bender's scored a goal. Show id, stadium, team1, team2 for just game 1012 
```
SELECT id,stadium,team1,team2
  FROM game
where id = 1012
```
### 3. The code below shows the player (from the goal) and stadium name (from the game table) for every goal scored. Modify it to show the player, teamid, stadium and mdate for every German goal. 
```
SELECT player, teamid, stadium, mdate
  FROM game JOIN goal ON (id=matchid)
where teamid = 'ger'
```
### 4. Use the same JOIN as in the previous question. Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
```
select team1, team2, player
from game join goal on (id=matchid)
where player like 'mario%'
```
### 5. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10 
```
SELECT player, teamid, coach, gtime
  FROM goal join eteam on (teamid = id)
 WHERE gtime<=10
```
### 6. List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach. 
```
select mdate, teamname  
from game join eteam on (team1=eteam.id)
where coach = 'fernando santos'
```
### 7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
```
select player
from game join goal on (id=matchid)
where stadium = 'National Stadium, Warsaw'
```
### 8. The example query shows all goals scored in the Germany-Greece quarterfinal. Instead show the name of all players who scored a goal against Germany.
```
SELECT distinct player
  FROM game JOIN goal ON matchid = id 
    WHERE (team1='GER' OR team2='GER') AND teamid <> 'ger'
```
### 9. Show teamname and the total number of goals scored.
```
SELECT teamname, count(teamid)
  FROM eteam JOIN goal ON id=teamid
 group by teamname
```
### 10. Show the stadium and the number of goals scored in each stadium.
```
select stadium, count(teamid)
from game join goal on id=matchid
group by stadium
```
### 11. For every match involving 'POL', show the matchid, date and the number of goals scored.
```
SELECT matchid, mdate, count(teamid)
  FROM game JOIN goal ON matchid = id 
 WHERE (team1 = 'POL' OR team2 = 'POL')
group by matchid, mdate
```
### 12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER' 
```
select matchid, mdate, count(teamid)
from game join goal on matchid=id
where (team1='ger' or team2='ger') and teamid = 'ger'
group by matchid, mdate
```
### 13. List every match with the goals scored by each team as shown. Sort your result by mdate, matchid, team1 and team2.

| mdate | team1 | score1  | team2 | score2 |
| --- |---| --- |---| --- |
| 1 July 2012 | ESP | 4 | ITA | 0 |
| 10 June 2012 | ESP | 1 | ITA | 1 |
| 10 June 2012 | IRL | 1 | CRO | 3 |

```
SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END )score1,
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
  FROM game LEFT JOIN goal ON matchid = id
group by mdate, matchid, team1, team2
```


Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'