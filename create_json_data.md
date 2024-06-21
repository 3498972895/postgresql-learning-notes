# Create JSON formate data

As we all known, the json format data is popular in web development. And it is common choice to use json to display infomation on web pages. If we can tranfrom data to json at the stage of data fetching from database, I think that's would be very useful.

So, today we will introduce the sufficient json relatived functions that it helps you deal with data easily.

## build an json object

I will quickly show you the useage of jsonb_build_object function. It is much simple to create an json object.

```sql
select jsonb_build_object(
    'title', 'Academy Dinosaur', 'length', 86
);
```
Output:
```json
{"title": "Academy Dinosaur", "length": 86}
```
Another example with Practical implications

```sql
SELECT jsonb_build_object(
    'title', title, 'length', length
) FROM film
ORDER BY length DESC;
```
Output:
```json
 {"title": "Muscle Bright", "length": 185}
 {"title": "Control Anthem", "length": 185}
 {"title": "Sweet Brotherhood", "length": 185}
```
This is a simple showcase, we also can combine the more complex useage, then recording the real data in jsonb_build_object function.


And there is an row_to_json function which make json more effective. The result with blew query is the same.

```sql
SELECT row_to_json(t)  FROM 
(
SELECT title, lenth FROM film
ORDER BY length DESC;
) as t
```

## build an json array

Sometimes, We can Omit the keys and only get the set of valuse  using the json array if we want the data more concise.

```sql
SELECT jsonb_build_array(
     title, length
) FROM film
ORDER BY length DESC;
```
Output:

```json
["Muscle Bright",185]
["Control Anthem",185]
["Sweet Brotherhood",185]
```
