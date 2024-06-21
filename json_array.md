# Working with json array

Today we will learn 3 simple functions about json array:

1. getting length from json array
2. destructing json array
3. destructing json array to text

We have some sample down blew:

```sql
CREATE TABLE person (
    id SERIAL PRIMARY KEY,
    info JSONB
);
```

```sql
INSERT INTO person (info)
VALUES
    ('{"name": "Alice", "age": 30, "pets": [{"type": "cat", "name": "Fluffy"}, {"type": "dog", "name": "Buddy"}]}'),
    ('{"name": "Bob", "age": 35, "pets": [{"type": "dog", "name": "Max"}]}'),
    ('{"name": "Charlie", "age": 40, "pets": [{"type": "rabbit", "name": "Snowball"}]}')
```

And if we want to get how many pets their masters have, so should use the jsonb_array_length function

```sql
SELECT info->>'name' as person_name,
jsonb_array_length(info->'pets') as pet_count
FROM person;
```

output:
|person_name|pet_count|
| ----------| ------- |
|Alice|2|
|Bob|1|
|Charlie|1|

The pets column is an json array, and we have method to destructure them into many rows by using jsonb_array_elements

```sql
select jsonb_array_elements(info->'pets') as pet from person;
```

And we will get 4 rows from data not grouped by their masters

output:
|pet|
| ---|
|{"type": "cat", "name": "Fluffy"}|
|{"type": "dog", "name": "Buddy"}|
|{"type": "dog", "name": "Max"}|
|{"type": "rabbit", "name": "Snowball"}|

and we have another functions named jsonb_array_elements_text which transfroms data to text format.