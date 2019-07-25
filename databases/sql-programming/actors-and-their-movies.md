# Actors and their Movies

https://www.interviewbit.com/problems/actors-and-their-movies/

Write a SQL Query to find the name of those movies where one or more actors acted in two or more movies.

### Output Schema

```
movie_title
```

NOTE:

1. Output column name has to match the given output schema.
2. Any name is the concatenation(without any delimiter) of first and last name if present
3. (E.g. if director_first_name is 'Alfred' and director_last_name is 'Hitchcock' then director_name is 'AlfredHitchcock')

### Example Output

```
movie_title
Vertigo
```

### Schema Design

![](https://s3-us-west-2.amazonaws.com/ib-assessment-tests/problem_images/sql_course.jpg)

### Solution
```sql
select movie_title
from movies where movie_id in
(select movie_id from movies_cast where actor_id in
(select actor_id from movies_cast group by actor_id having count(movie_id)>=2));
```
