# Select within select

## Tables
world

| name | continent |area | population | gdp |
| --- |---| --- |---| --- |
| Afghanistan |Asia | 652230 | 25500100 | 20343000000 |
| Albania |Europe | 28748 | 2831741 |12960000000 |
| Algeria |Africa | 2381741 | 37100000 | 188681000000 |
| Andorra |Europe | 468 | 78115 |3712000000 |
| Angola | Africa | 1246700 | 20609294 | 100990000000 |

### 1. List each country name where the population is larger than that of 'Russia'.
```
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name like 'Russia')
```
### 2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```
select name
from world
where continent = 'europe' and  gdp/population > (select gdp/population from world where name = 'united kingdom')
```
### 3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```
select name, continent
from world 
where continent in (select continent
from world
where name in ('argentina', 'australia')
)
order by name 
```
### 4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.
```
select name, population
from world
where population > (select population from world where name = 'canada') and population <
(select population from world where name = 'poland')
```
### 5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany
```
select name, concat(round(round(population/(select population from world where name = 'germany'), 2)*100), '%') as percentage
from world
where continent = 'europe'
```
### 6. Which countries have a GDP greater than every country in Europe? (Some countries may have NULL gdp values)
```
select name
from world
where gdp >= all (select gdp from world where gdp >0 and continent = 'europe' ) and continent <> 'europe'
```
### 7. Find the largest country (by area) in each continent, show the continent, the name and the area:
```
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
```
### 8. List each continent and the name of the country that comes first alphabetically.
```
select continent, top 1 name
from world
order by name
```