# Json_modify

This article will talk about the json updating, deleting.

1) updating values in json

There are two types operation of updating

1. add something new in json.
2. replace old with something new in json.

Both problems have different functions to deal with themselves.

The function named jsonb_insert is suitable for the situation 1. When we want to add something new in json, we should provide the targe what we will insert, the path where we insert, and the value what we will use. So the function syntax is

```sql

jsonb_insert(
   target jsonb, 
   path text[], 
   new_value jsonb, 
   [insert_after boolean]
) → jsonb

```
The insert_after is a option whose boolean's value decides whether the new value will be inserted after the path. The default boolean's value is false, meaning that the new value  the new_value_jsonb provided will be inserted before the path.

The jsonb_insert only support insert new but not support the repalcment action.

1. if target is an json array

1.1 in top level 

```sql
SELECT jsonb_insert('[1,2,3]', '{0}', '0');
```

output:
```json
[0,1,2,3]
```

1.2 if nested:

```sql
SELECT 
  jsonb_insert(
    '[1,2,[4,5],6]', '{2,0}', '3'
  );
```
output:

```json
[1,2,[0,4,5],6]
```

2. if target is an json object

2.1 in top level:


```sql
SELECT 
  jsonb_insert('{"name": "John"}', '{age}', '2');
```

2.2 if nested

```sql
SELECT 
  jsonb_insert(
    '{"name":"John Doe", "address" : { "city": "San Francisco"}}', 
    '{address,state}', 
    '"California"'
  );

```
3. if target is mixed

```sql
SELECT 
  jsonb_insert(
    '{"name": "John", "skills" : ["PostgreSQL", "API"]}', 
    '{skills,1}', '"Web Dev"'
  );
```

When we use this to update new:

```sql
UPDATE 
  employee_profiles 
SET 
  profiles = jsonb_insert(
    profiles, '{skills,0}', '"Web Dev"'
  ) 
WHERE 
  id = 1;
```
we can infer that the 'skills' is an array in json, we will insert 'Web Dev' in position 0.

There had another function named jsonb_set which is similar with jsonb_insert. But the value will be inserted when the value no exists in type of json_object target while the value will be updated when the value exists. 

2) deleting values in json

The operator(-) and operator(||) has limited on updating and deleting becausee they both can only be used on top level on json data. And Though they can finish the task, but we should operate the modifying data firstly and then union with the whole json.

If we have the fake data, then we want to delete the 'webcam' property in 'extras' nested in 'attributes'  json column in products table where id =1.
```json
{
  "id": 1,
  "name": "Laptop",
  "specs": {
    "cpu": "Intel i7",
    "gpu": "Nvidia GTX 1650",
    "ram": null,
    "extras": { "webcam": null, "bluetooth": 'huawei xzx', "fingerprint_reader": true }
  }
}
```
we should do 

```sql
UPDATE products SET attributes = attributes||
jsonb_build_object('specs',
attributes#>'{specs}'||
 jsonb_build_object('extras',(attributes#>'{specs,extras}')::jsonb-'webcam'))
WHERE attributes->>'id'='1';
```
So it is so complex and difficult to finish this and I took 20 minitus to check grammer because it has a lot of thing caring. Then the deleting sames too if our operation is not on the top level of json.

And it has a trick, first We set the value which will be deleting to null. Then we use jsonb_strip_nulls function and it receive a param whose type is jsonb and remove all null value recursively.

First,set the removing data to null. Then executing the following sql.

```sql
update products set attributes = jsonb_strip_nulls(attributes)
where attributes->>'id'='1';
```