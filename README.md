# Coding Challenge Part 2

## Schema

<img src="Coding Challenge Part 2.png">

## Queries

### (3) available classes
```
select c.id as class_id, c.name as class_name, c.area_of_interest, c.capacity, l.name as lecturer_name, l.rating, 
case when ce.id is not null then true else false end as already_enrolled
from classes c
join lecturer l on c.lecturer_id=l.id
left join class_enrollment ce on ce.class_id=c.id and ce.student_id=(<id>)
where c.area_of_interest in (<areas>)
order by l.rating desc -- assumes higher rating values are better
```

### (5) classes where # of students is < 50% capacity
```
select c.id as class_id, c.name, c.lecturer_id as lecturer_id, enrollment_counts.num_students, c.capacity
from (select ce.class_id, count(*) as num_students from class_enrollment ce group by ce.class_id) enrollment_counts
join classes c on enrollment_counts.class_id=c.id
where enrollment_counts.num_students < (0.5*c.capacity)
```


  
## Original Specifications

--- Task 2
--- data modeling ----
How would you design a database for this application:
- need to build an app to allow university students to sign up for classes
- given tables: students (id, name, email, admission year, current_status); lecturer (id, name, email, start year, end year, rating, current_status);

Functionality requirements:
1. As a lecturer I want to be able to publish a new class, limiting capacity for a specific semester
2. As a lecturer I want to be able to see how many students signed up for my class and be able to distribute class materials to them over email
3. As a student I want to be able to find available classes filtered by area of my interest ordered by the rating of lecturer. Please add a flag indicating if the student has signed up for the class already. 
4. As administrator I want to make sure that a single class does i snot subscribed over capacity
5. As admin I want to find classes where the number of students is less than 50% of capacity


Please provide a DB schema and queries for the requirements #3 and #5