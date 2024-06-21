# Constraints

This is an article about the grammer of constaints in postgresql rather than telling the concept of constraints.

There are so many situations that we can generate an constraint. 


## primary key

### Adding a primary key
1) When creating table with a primary key consists of one column

```sql
CREATE TABLE table_name (
  column_1 data_type PRIMARY KEY, 
);
```

2) when creating table with a primary key consists of mutiple columns

```sql
CREATE TABLE table_name (
  column_1 data_type, 
  column_2 data_type,
  PRIMARY KEY(column_1, column2, ...)
);
```

3) Adding a primary key to an existing table

```sql
ALTER TABLE table_name 
ADD PRIMARY KEY (column1, column2, ...);
```

### Autoincrement

postgresql's primary key is not autoincrement by default.So here are two ways to achieve it.

1) adding SERIAL dataType on  primary key 
2) using GENERATED { ALWAYS | BY DEFAULT } AS IDENTITY constraint

### Drop a primary key

Sometimes, we need remove a primary for some reasons. These reasons include that we need add a new one or  change to another one.

```sql
ALTER TABLE table_name 
DROP CONSTRAINT primary_key_constraint;
```
The name of primary key constraint is table-name_pkey by default. And we can also type '\\d table-name' to check the structure of table for getting a name of constraint.

## Foreign key

### add a Foreign key

1) When creating a table using REFERENCES keyword

```sql
CREATE talbe_name (
    columm1 data_type,
    column2 data_type REFERENCES table_refer(column_refer);
)
```
2) when creating a table using CONSTRAINT

```sql
CREATE table_name (
  column1 data_type,
  column2 data_type,
  CONSTRAINT fk_custom_name
      FOREIGN KEY(customer_id) 
        REFERENCES customers(customer_id)
)
```
Specify the name for the foreign key constraint after the CONSTRAINT keyword. The CONSTRAINT clause is optional. If you omit it, PostgreSQL will assign an auto-generated name.

3) adding to an existing table


```sql
ALTER TABLE child_table 
ADD FOREIGN KEY (fk_columns) 
REFERENCES table_refer (column_refer);
```
We should drop or check the old constraint before we do this.

### Drop a Foregin Key

```sql 
ALTER TABLE table_name
DROP CONSTRAINT constraint_fkey;
```

## CASCADE On Deleting

Most of times We prefer using the delete casade more frequently than update cascade in practice. Normaly, there are different three ways to  tackle delete operation on parent table.

### with NO ACTION

In this situation, we have no extra setting on Foreign key constraint, So we can not execute delete or update oepraion on them.

### SET NULL

```sql
FOREIGN KEY(fk_column) 
	  REFERENCES table_refer(column_refer)
	  ON DELETE SET NULL
```
After we do this on foreign constraint, We can easily delete the row from table_refer, then the referencing row will be set to NULL.

### DELETE CASCADE

```sql
FOREIGN KEY(fk_column) 
	  REFERENCES table_refer(column_refer)
	  ON DELETE CASCADE
```
The referencing row will disappear after delete oepration on table_refer.

### SET DEFAULT 

like SET NULL, We can also set the another value By 'ON DELETE CASCADE SET DEFAULT'  When deleting operation has happened.


## CHECK CONSTRAINT

Sometimes, we may have some requirements on any fields, liking the salary must be greater than 0, the age on employees could be less than 65 ages. That is a useful check methods using CHECK CONSTRAINT to check  values that when we  execute udpate or insert oepration.


### add CHECK CONSTRAINT 

1) when creating table after column
```sql
CREATE TABLE table_name(
  column1 data_type,
  column2 data_type CHECK(conditions)
);
```
2) when creating table within individual record

```sql
CREATE TABLE table_name(
  column1 data_type,
  column2 data_type,
  CHECK(condition)
);
```
3) add to existing table

```sql
ALTER TABLE table_name ADD CHECK (conditions);
```

### Removing Constraint

```sql
alter table drop constraint column_name_check;
```

## UNIQUE Constraint

The way of 'UNIQUE constraint' operaton is so similar with CHECK constraint that we only need to change the keywords to UNIQUE. And extra Tips here is that we can set mutiple columns within one unique group.

```sql
CREATE TABLE table_name(
  column1 data_type,
  column2 data_type,
  UNIQUE(column1,column2);
);
```

## NOT NULL Constraint

To control whether or not a row is NULL, we use NOT NULL constraint.

### ADD NOT NULL CONSTRAINT

1) add NOT NULL when creating

```sql
create table table_name(
  column1 data_type,
  column2 data_type NOT NULL
)
```

2) add to existing table 

```sql
alter table alter column SET NOT NULL;
```
### Removing Constraint

```sql
alter table alter column drop NOT NULL;
```



## DEFAULT constraint

The operation using 'DEFAULT' is similar with NOT NULL. You need add the default value after DEFAULT keywords.