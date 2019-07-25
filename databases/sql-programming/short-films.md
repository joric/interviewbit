# Short Films

https://www.interviewbit.com/problems/short-films/

Write a SQL Query to find those lowest duration movies along with the year, director's name(first and last name combined), actor's name(first and last name combined) and his/her role in that production.

### Output Schema

```
movie_title,movie_year,director_name,actor_name,role
```

NOTE:

1. Output column name has to match the given output schema.
2. Any name is the concatenation(without any delimiter) of first and last name if present
3. (E.g. if director_first_name is 'Alfred' and director_last_name is 'Hitchcock' then director_name is 'AlfredHitchcock')

### Example Output

```
movie_title,movie_year,director_name,actor_name,role
Vertigo,1958,AlfredHitchcock,JamesStewart,JohnFerguson
```

### Schema design

![](https://s3-us-west-2.amazonaws.com/ib-assessment-tests/problem_images/sql_course.jpg)

## Solution

```sql
select m.movie_title, m.movie_year,
concat(d.director_first_name, d.director_last_name) as director_name,
concat(a.actor_first_name, a.actor_last_name) as actor_name, mc.role
from
movies m join movies_cast mc on m.movie_id = mc.movie_id
join movies_directors md on md.movie_id = m.movie_id
join directors d on md.director_id = d.director_id
join actors a on mc.actor_id = a.actor_id
order by m.movie_time asc limit 1
```
