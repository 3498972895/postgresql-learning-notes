# Group By and Aggregate Functions

The GROUP BY divides the rows in different groups. And We can only manipulate them as a individual rather than an single row when using GROUP BY clause.

```sql
SELECT column_x, aggregate_function(column_name_2)
FROM
GROUP BY column_name_2
```
The AGGREGATE_Funcions we frequently  use are SUM, COUNT , MAX, MIN and AVG.  

Imagine We have a table named KPI used for caculate the scores of all employees in different department.

So, the sql can be write: 

```sql
SELECT depart_name, AVG(scores) AS avg_scores
FROM kpi
GROUP BY depart_id
WHERE depart_name<>services;
```

The purpose of depart_name is to distinguish different group, and the reuslt may be like this:

|depart_name| avg_scores|
| --------- | ----- |
|market|88|
|technology|89|
|public| 87|


The GROUP BY often used with aggregate_function together,

## HAVING clause

The WHERE takes effect on row level as we have already seen before, but the HAVING clause only works on Group level.  It's useful when we make any constraints on Group.

For example, if we want to filter the avg_score blew 60 deparment, we can using:

```sql
SELECT depart_name, AVG(scores) AS avg_scores
FROM kpi
GROUP BY depart_id
HAVING AVG(scores)< 60
WHERE depart_name <> services;
```
