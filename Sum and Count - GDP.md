# Sum and Count - GDP

## Tables
world

| name | continent |area | population | gdp |
| --- |---| --- |---| --- |
| Afghanistan |Asia | 652230 | 25500100 | 20343000000 |
| Albania |Europe | 28748 | 2831741 |12960000000 |
| Algeria |Africa | 2381741 | 37100000 | 188681000000 |
| Andorra |Europe | 468 | 78115 |3712000000 |
| Angola | Africa | 1246700 | 20609294 | 100990000000 |

### 1. Show the total population of the world. 
```
SELECT SUM(population)
FROM world
```
### 2. List all the continents - just once each.
```
select distinct continent
from world
```
### 3. Give the total GDP of Africa 
```
select sum(gdp)
from world
where continent = 'africa'
```
### 4. How many countries have an area of at least 1000000
```
select count(name)
from world
where area > 1000000
```
### 5. What is the total population of ('Estonia', 'Latvia', 'Lithuania') 
```
select sum(population)
from world
where name in ('Estonia', 'Latvia', 'Lithuania')
```
### 6. For each continent show the continent and number of countries.
```
select continent, count(name)
from world
group by continent
```
### 7. For each continent show the continent and number of countries with populations of at least 10 million.
```
select continent, count(name)
from world
where population > 10000000
group by continent
```
### 8. List the continents that have a total population of at least 100 million.
```
select continent
from world
group by continent
having sum(population) > 100000000
```
