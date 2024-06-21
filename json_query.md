# JSON QUERY

Now, We have some data stored with 'info' column in person table.

```json
{"name": "Alice", "age": 30, "pets": [{"type": "cat", "name": "Fluffy"}, {"type": "dog", "name": "Buddy"}]}
{"name": "Bob", "age": 35, "pets": [{"type": "dog", "name": "Max"}]}
{"name": "Charlie", "age": 40, "pets": [{"type": "rabbit", "name": "Snowball"}]}
```
If We want all names, we can write:
```sql
SELECT info->'name' FROM person;
```
However, if want all pets'name, we can not achieve this. Because all operators about json support us only get the first level data, and have no  method to extract info deeper.

But fortunately, Postgresql provides the jsonb_path_query function which have the power of the normal oeprators but also have the ability to extract the info deeply solving the problem we metioned.

```sql
SELECT jsonb_path_query(info,'$.pets[*].name') FROM person;
```
Output: 

|name|
| --- |
|"Fluffy"|
|"Buddy"|
|"Max"|
|"Snowball"|

But here has an problem with those data. If we look at those json carefully, we will see that "Fluffy" and "Buddy" are blong to one person. So the another function is useful to trackle this.

```sql
SELECT jsonb_path_query(info,'$.pets[*].name') FROM person;
```
Output: 

|name|
| --- |
|["Fluffy","Buddy"]|
|["Max"]|
|["Snowball"]|

finally, we can also use multiple functions to construct the json data.

```sql
SELECT jsonb_build_object('master', info->'name',
'pets', jsonb_path_query_array(info,'$.pets[*].name')
) master FROM person;
```
then output:

|master|
| ----- |
|{"master":"Alice","pets":["Fluffy","Buddy"]}
|{"master":"Bob","pets":["Max"]}
|{"master":"Charlie","pets":["Snowball"]}