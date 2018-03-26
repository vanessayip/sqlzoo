# Joins - movies
This database features two entities (movies and actors) in a many-to-many relation. Each entity has its own table. A third table, casting , is used to link them. The relationship is many-to-many because each film features many actors and each actor has appeared in many films.

## Tables
movie

| id | title | yr | director | budget | gross |
| --- |---| --- |---| --- | --- |
| pk |  |  | fk |  |  |

Actor

| id | name |
| --- |---|
| pk |  |

Casting

| movieid | actorid | ord |
| --- |---| --- |
| pk, fk | pk, fk |  |

### 1. List the films where the yr is 1962 [Show id, title]
```
SELECT id, title
 FROM movie
 WHERE yr=1962
```
### 2. Give year of 'Citizen Kane'.
```
select yr
from movie
where title ='citizen kane'
```
### 3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
```
select id, title, yr
from movie
where title like 'star trek%'
```
### 4. What id number does the actor 'Glenn Close' have?
```
select id
from actor
where name = 'glenn close'
```
### 5. What is the id of the film 'Casablanca'
```
select id
from movie
where title = 'casablanca'
```
### 6. Obtain the cast list for 'Casablanca'.

what is a cast list?
Use movieid=11768, (or whatever value you got from the previous question)
```
select name
from casting join actor on actorid = id
where movieid = 11768
```
### 7. Obtain the cast list for the film 'Alien'
```
select name
from (movie join casting on id = movieid) join actor on actorid = actor.id
where title = 'alien'
```
### 8. List the films in which 'Harrison Ford' has appeared
```
select title
from (actor join casting on id=actorid) join movie on movieid = movie.id
where name = 'harrison ford'
```
### 9. List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]
```
select title
from (actor join casting on id=actorid) join movie on movieid = movie.id
where name = 'Harrison ford' and ord <> '1'
```
### 10. List the films together with the leading star for all 1962 films.
```
select title, name
from (movie join casting on id = movieid) join actor on actorid = actor.id
where yr = '1962' and ord = '1'
```
### 11. Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
```
SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
where name='John Travolta'
GROUP BY yr
HAVING COUNT(title)=(SELECT MAX(c) FROM
(SELECT yr,COUNT(title) AS c FROM
   movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
 where name='John Travolta'
 GROUP BY yr) AS t
)
```
### 12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.
```
select title, name
from movie join casting on (movieid = movie.id and ord = '1')
  join actor on (actorid=actor.id)
where movie.id in (
SELECT movieid FROM casting
WHERE actorid IN (
  SELECT id FROM actor
  WHERE name='Julie Andrews'))
```
### 13. Obtain a list, in alphabetical order, of actors who've had at least 30 starring roles.
```
select name
from movie join casting on (movie.id = movieid) join actor on (actorid = actor.id)
where ord = '1'
group by name
having count(name) >= 30
order by name asc
```
### 14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.
```
select title, count(actorid)
from movie join casting on (movie.id = movieid)
where yr = '1978'
group by title
order by count(actorid) desc, title asc
```
### 15. List all the people who have worked with 'Art Garfunkel'.
```
select name
from casting join actor on (actorid = actor.id)
where movieid in (
select movieid
from casting join actor on (actorid = actor.id)
where name = 'art garfunkel') and name <> 'art garfunkel'
```
