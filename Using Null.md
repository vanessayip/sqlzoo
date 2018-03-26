# Using Null
The school includes many departments. Most teachers work exclusively for a single department. Some teachers have no department.
## Tables
Teacher

| id | dept | name | phone | mobile |
| --- |---| --- |---| --- |
| 101 | 1 | Alice | 1234567890 | 0987654321 |
| 102 | 1 | Bob | 2345678901 | 9876543210 |
| 103 |  | Charles | 3456789012 | 8765432109 |
| 104 | 2 | Daniella | 4567890123 | 7654321098 |

Dept

| id | name |
| --- |---|
| 12 | some-company |

### 1. List the teachers who have NULL for their department.
Why we cannot use =
```
select name
from teacher
where dept is null
```
### 2. Note the INNER JOIN misses the teachers with no department and the departments with no teacher.
```
SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)
```
### 3. Use a different JOIN so that all teachers are listed.
```
select teacher.name, dept.name
from teacher left join dept on (teacher.dept = dept.id)
```
### 4. Use a different JOIN so that all departments are listed.
```
select teacher.name, dept.name
from teacher right join dept on (teacher.dept = dept.id)
```
### 5. Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given. Show teacher name and mobile number or '07986 444 2266'
```
select teacher.name, coalesce(mobile, '07986 444 2266')
from teacher
```
### 6. Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where there is no department.
```
select teacher.name, coalesce(dept.name, 'None')
from teacher left join dept on (teacher.dept = dept.id)
```
### 7. Use COUNT to show the number of teachers and the number of mobile phones.
```
select count(name), count(mobile)
from teacher
```
### 8. Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.
```
select dept.name, count(teacher.name)
from teacher right join dept on (dept = dept.id)
group by dept.name
```
### 9. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2 and 'Art' otherwise.
```
select name,
case when dept = 1 then 'Sci'
when dept = 2 then 'Sci'
else 'Art'
end
from teacher
```


