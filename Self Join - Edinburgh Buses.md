# Self join - Edinburgh Buses

## Tables
Stops

id  | name 
--- | --------- 
123 | some-name 

Routes

| num | company | pos | stop |
| --- |---| --- |---|  
| 12 | some-company | 1 | 150 |

### 1. How many stops are in the database
```
select count(id)
from stops
```
### 2. Find the id value for the stop 'Craiglockhart'
```
select id
from stops
where name = 'Craiglockhart'
```
### 3. Give the id and the name for the stops on the '4' 'LRT' service.
```
select id, name
from stops join route on (id = stop)
where num = '4' and company like '%LRT%'
```
### 4. The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these stops have a count of 2. Add a HAVING clause to restrict the output to these two routes.
```
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
having count(*) > 1
```
### 5. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.
```
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
WHERE a.stop=53 and b.stop = 149
```
### 6. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'
```
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart'and stopb.name='London Road'
```
### 7. Give a list of all the services which connect stops 
```
SELECT a.company, a.num
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
WHERE a.stop=115 and b.stop = 137
group by a.company, a.num, a.stop, b.stop
having count(a.num) >= 1
```
### 8. Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'
```
select a.company, a.num
from route a join route b on (a.company = b.company and a.num = b.num)
join stops stopa on (a.stop =stopa.id)
join stops stopb on (b.stop = stopb.id)
where stopa.name = 'Craiglockhart' and stopb.name = 'Tollcross'
```
### 9. Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.
```
select stopb.name, a.company, a.num
from route a join route b on (a.company = b.company and a.num = b.num)
join stops stopa on (a.stop =stopa.id)
join stops stopb on (b.stop = stopb.id)
where stopa.name = 'Craiglockhart' and a.company = 'LRT'
```
### 10. Find the routes involving two buses that can go from Craiglockhart to Sighthill.
Show the bus no. and company for the first bus, the name of the stop for the transfer,
and the bus no. and company for the second bus.
```
select DISTINCT c.num, c.company, c.name, d.num, d.company
from 
(select a.num, a.company, stopa.name as an, stopb.name
from (route a join route b on (a.company = b.company and a.num = b.num)
join stops stopa on (a.stop = stopa.id)
join stops stopb on (b.stop = stopb.id)) 
where stopa.name = 'Craiglockhart') as c
join 
(select a.num, a.company, stopa.name, stopb.name as bname
from (route a join route b on (a.company = b.company and a.num = b.num)
join stops stopa on (a.stop = stopa.id)
join stops stopb on (b.stop = stopb.id)) 
where stopb.name = 'Sighthill') as d
on c.name = d.name
ORDER BY ABS(c.num), c.name 
```
