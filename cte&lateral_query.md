# CTE 

The CTE is common table expression which creates a temporary set for the convinience of later query.

In my daily manipulating database, I can not encounter the situation that requires me to use it. In other word, The both CTE and Lateral query are not be uesd frequently from a web developer respective not a professional database manager.

```sql
WITH cte_table_name(column1, column2, ...) AS (
    SELET ...
);
```
The section after AS keyword in the parentheses is a result set created by a query. We can save the data from a query and be used for join operation.

```sql
SELECT ... from table_name join cte_table_name on ...condition;
```
# Lateral Query

Another way to creating a temporary data is LATERAL Subquery with using LATERAL keyword.



The LATERAL which wraps a query can be used as a result set.

```sql
select t_id, name,tc.class_name from teachers as t,LATERAL (
    SELECT class_name from classess where class_id  in 
    SELECT class_id from classess_teachers as c where t.t_id = c.t_id
) as tc;
```

In this little complex query, we use the LATERAL to create an temprary data join the teachers table for getting a  teacher's teaching classes set.

The advantage with LATERAL is that it can utilize the fields outside the LATERAL(t.t_id) Nonetheless the subquery without LATERAL will receive the error report form data query.