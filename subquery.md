# SubQuery

## SubQuery Basic

The subquery is multiple-table query. And the purpose of subquery is to make query simple although making query slowly.


## Example_1

We have two tables:
    countries: country_id, country_name
    cities: country_id, city_name

If we want to search all cities which the china includes, we could be write:

```sql
SELECT country_id, city_name 
FROM countries JOIN cities USING('country_id') 
WHERE country_name = 'china';
```

the subquery version:

```sql
SELECT country_id,country_name FROM cities WHERE country_id = (
    SELECT country_id FROM countries WHERE country_name = 'china';
)
```

The speed of join-query is faster than the subquery usually.

## Example_2

Let us consider more complex subquery when result of subquery returns more than one rows.

We have three tables:
    category:category_id,category_name
    film_category:film_id,category_id
    film:film_id,film_name,film_length

From the structrue of tables, We can see the film_category is the intermediate table. Our query is how to find all the name of film in 'action' category.

This is not easily using query without subquery, and we can split the question into many steps:

1. we should get the category_id when the category_name='action', suppose result is  category_id=3
```sql
select category_id from category where category_name="action";
```
2. Then finding all the film_id satisfying the category_id=3 the result set of film_id is (1,3,5,8)
```sql
select film_id from film_category where category_id = 3;
```
3. searching all the film_name where film_id in (1,3,5,8)

```sql
select film_name from film where film_id in (1,3,5,8);
```

The last step is to connect the queries and replace the fake data with queries.

```sql
SELECT film_name FROM film WHERE film_id IN (
    SELECT film_id FROM film_category  WHERE category_id = (
        SELECT category_id form category WHERE category_name = 'action'
    )
)
```

It may  be not the best choice but it's a solution.


## Subquery Operators

There are there type of Subquery Operators in Postgresql:ANY,ALL,EXISTS.

Suppose we have a employees table and managers table.

1) We can get all the employees whose salary is greater than all manager's by All.

```sql
select employees_id,employees_name,employees.salary from employees where salary > ALL(
    select salary from managers
)
```
2) We can get all the employees whose salary is greater than least one manager's by ANY

```sql
select employees_id,employees_name,employees.salary from employees where salary > ANY(
    select salary from managers
)
```
3) We can Seach all films which has been classified.

Subquery which the EXISTS Wrapped returns the value of boolean.

```sql
SELECT film_id,film_name from film as f where EXISTS(
  select 1 from film_category  as fc where f.film_id  = fc.film_id
)
```
