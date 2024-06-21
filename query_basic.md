# Query Basic

In this chapter, We will talk about the fundamental query in postgresql. 

It is  not cover all details about query, this chapter only focus the Operators and common operations in most situations of queries We often use. 

## Teaching by example

### Create Table
first, We will use 'create table' clause to setup our playground for practicing content we will learn.

```sql
CREATE TABLE teachers (
    id bigserial,
    first_name varchar(25),
    last_name varchar(50),
    school varchar(50),
    hire_date date,
    salary numeric
);
```
This is how we can create a table, giving some necessary infos likes table_name:teachers, some fields with their data_type like: id columns with bigserial dataType.

It doesn't matter if you are not unfamiliar with dataType define system. Generally, we can store numbers, date, characters.

### Fill Some Data

We only created a Empty table before, now It's time to write some data into it.

```sql
INSERT INTO teachers(first_name, last_name, school, hire_date, salary)
VALUES ('Janet', 'Smith', 'F.D.Roosevelt HS', '2011-10-30', 36200),
       ('Lee', 'Reynolds', 'F.D. Roosevelt HS', '1993-05-22', 65000),
       ('Samuel', 'Cole', 'Myers Middle School', '2005-08-01', 43500),
       ('Samantha', 'Bush', 'Myers Middle School', '2011-10-30', 36200),
       ('Betty', 'Diaz', 'Myers Middle School', '2005-08-30', 43500),
       ('Kathleen', 'Roush', 'F.D. Roosevelt HS', '2010-10-22', 38500);
```

Here, We insert into teacher many rows. Of course, you can insert arbitrary counts of rows after VALUES words. But Be carefully, We must write with right order according to the fileds sequence We specific before like first_name, last_name, school...

If We don't specifc the sequence, We must write based on the fields When created.

### Query Basic

Usually, We can query all data without any conditions. And the asterisk(*) sysmbols all columns.

```sql
SELECT * FROM teachers;
```

But for better conventions, We should write the columns name directly. That's important for another to understand our table's structure quickly.

```sql
SELECT
id, first_name, last_name, school, hire_date, salary 
FROM teachers;
```


### DISTINCT

Here, there are two words we often use in most queries.

DISTINCT helps us filter the unique columns info. For Example, We have already inserted six rows data. But What should We do If We want to know which schools the teachers come from  in data. 

```sql
SELECT DISTINCT school from teachers;
```
The results should be: F.D.Roosevelt HS and Myers Middle School

### ORDER BY

The order of each row in query result is depend on the order we inserted. But We can use Order By to adjust the order of result.

For example, We sort the data by salary ASC or DESC.

```sql
SELECT 
id, first_name, last_name, school, hire_date, salary 
FROM teachers 
ORDER BY salary ASC.
```


## Operators

When we query some data, we will attach some conditions to meet our query needs. For example, we get all teachers whose salary should be great than a certain value.

```sql
select * from teachers WHERE salary > 30000 AND school = 'Myers Middle school';
```
We put some conditions after WHERE. combining them by AND means the rows should pass all of them,combining them by OR means the rows only pass one of them. 

Here are MATH Operator:

|Operator| Function| Example|
| -------| --------| -------|
|=       |Equal to |WHERE school ='Myers Middle school|
|<> or !=|Not equalto|WHERE school <> 'Myers Middle school|
| >      | Greater than| WHERE salary > 20000|
|<       | Less than | WHERE salary <  2000|
|>=| Greater than or equal to | WHERE salary >= 2000| 
|<=| Less than or equal to | WHERE salary <= 2000| 


Here are some special operator

|Operator| Function| Example|
| -------| --------| -------|
|Between|Within a range | WHERE salary BETWEEN 20000 AND 30000|
|IN | Match oneof set of values| WHERE id IN (1,2,3)|
|LIKE| Match Patterns| WHERE school LIKE '%school'|
|ILIKE| Match Patterns| WHERE school ILIKE '%school'|
|NULL| match the nullable columns| WHERE name NOT NULL|
here has a negate a operator:NOT, for example, the NOT LIKE is equal to ILIKE.


