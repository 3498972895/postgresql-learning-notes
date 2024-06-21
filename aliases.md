# Aliases

Every database product like mysql, mongodb, postgresql support aliases. We can set aliases with column or table to avoid the naming conflict.

For example, in self-join senario:

```sql
SELECT select_list
FROM table_name AS t1
INNER JOIN table_name AS t2 ON join_predicate;
```

Of course, you can set column aliase with As too:

```sql
SELECT column_name AS alias_name 
FROM 
teachers;
```

In postgresql, you can omit the AS after the target you want to rename with.



