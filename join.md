# Joining Tables In Ralational Database

It's easy to understand the join operations in two tables. Most of times when we talk about join, We are refering to inner join.

This is not a article  to learn the concept of JOIN, but a article for reviewing.

We don't talk about CROSS JOIN(Cartesian product) as the less commonly used in web develepment.
## Inner Join

```sql
SELECT * from table_a JOIN table_b
ON table_a.key_colunm = table_b.key_column;
```

If the name of key_column from table_a is same with the name of key_column from table_b, We can omit the ON clause and replace it by USING :

```sql
SELECT * from table_a JOIN table_b USING key_column;
```

## LEFT JOIN AND RIGHT JOIN

The inner join shows the result only satisfying the condition based ON clause.

The difference lies in how LEFT JOIN and RIGHT JOIN handle unmatched records. The left or right join display the all rows from one side depending on the join is left or right. And the rows of talbe on the another side will display NULL if it not match the condition.

### Understanding by Example 

employees
| id | name | depart_id|
| ---| -----| ---------|
|1 |tom|1a|
|2 |jerry|2b|
|3|spike|3c|
|4|newcomer|4b|


deparments

|depart_id|depart_name|
| ---| -----|
|1a|home|
|2b|wall holes|
|3c|outside|
|5e|somewhere|


The result of the JOIN operation will only display the rows that satisfy the condition specified in the ON clause.
| id | name | depart_id|depart_name|
| ---| -----| ---------| ----------|
|1 |tom|1a|home|
|2 |jerry|2b|wall holes|
|3|spike|3c|outsize|


the  result of left join.

| id | name | depart_id|depart_name|
| ---| -----| ---------| ----------|
|1 |tom|1a|home|
|2 |jerry|2b|wall holes|
|3|spike|3c|outsize|
|4|newcomer|4b||

the result of right join.

| depart_id| depart_name | id|name|
| ---| -----| ---------| ----------|
|1a |home|1|tom|
|2b |wall holes|2|jerry|
|3c|outside|3|spike|
|5e|somewhere|||

## Full Outer Join

The left or right join is kind of Outer join. Full Outer Join,
which combines the both left and right join, is seldom be used.

the result of FULL OUTER JOIN

| id | name | depart_id|depart_name|
| ---| -----| ---------| ----------|
|1 |tom|1a|home|
|2 |jerry|2b|wall holes|
|3|spike|3c|outsize|
|4|newcomer|||
|||5e|comewhere|


## NATURAL JOIN

The NATURAL can be added before [LEFT,RIGHT,JOIN], which implicits the condition based on the equality of common columns.

```sql
SELECT select_list
FROM table1
[INNER, LEFT, RIGHT] JOIN table2 
   ON table1.column_name = table2.column;
```

equals to when both have same columns:

```sql
SELECT select_list
FROM table1
NATURAL LEFT JOIN table2;
```