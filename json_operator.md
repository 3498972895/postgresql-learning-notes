# Json Opearators

There are a lot of operators for effectively operating with json data_type.

In Postgersql, there two json data_type:JSON and JSONB.

JSON is text and JSONB is binary format. The former of performance is slower than the later.

Here, We will introduce them by exaple. 


```sql
create table products(
    id serial PRIMARY KEY,
    data JSONB
)

INSERT INTO products (data)
VALUES
    ('{
        "name": "iPhone 15 Pro",
        "category": "Electronics",
        "description": "The latest iPhone with advanced features.",
        "brand": "Apple",
        "price": 999.99,
        "attributes": {
            "color": "Graphite",
            "storage": "256GB",
            "display": "6.1-inch Super Retina XDR display",
            "processor": "A15 Bionic chip"
        },
        "tags": ["smartphone", "iOS", "Apple"]
    }'),
    ('{
        "name": "Samsung Galaxy Watch 4",
        "category": "Electronics",
        "description": "A smartwatch with health tracking and stylish design.",
        "brand": "Samsung",
        "price": 349.99,
        "attributes": {
            "color": "Black",
            "size": "42mm",
            "display": "AMOLED display",
            "sensors": ["heart rate monitor", "ECG", "SpO2"]
        },
        "tags": ["smartwatch", "wearable", "Samsung"]
    }'),
    ('{
        "name": "Leather Case for iPhone 15 Pro",
        "category": "Accessories",
        "description": "Premium leather case for iPhone 15 Pro.",
        "brand": "Apple",
        "price": 69.99,
        "attributes": {
            "color": "Saddle Brown",
            "material": "Genuine leather",
            "compatible_devices": ["iPhone 15 Pro", "iPhone 15 Pro Max"]
        },
        "tags": ["phone case", "accessory", "Apple"]
    }'),
    ('{
        "name": "Wireless Charging Pad",
        "category": "Accessories",
        "description": "Fast wireless charger compatible with smartphones and smartwatches.",
        "brand": "Anker",
        "price": 29.99,
        "attributes": {
            "color": "White",
            "compatible_devices": ["iPhone", "Samsung Galaxy", "Apple Watch", "Samsung Galaxy Watch"]
        },
        "tags": ["accessory", "wireless charger"]
    }')
RETURNING *;
```

1) Operator(->/->>) example

This -> allows us to extract a JSONB to JSONB while by providing a key.
This ->> allows us to extract a JSONB to text by providing a key.

```sql
SELECT data->'name' as product_name FROM products;
SELECT data->>'name' as product_name FROM products;
```
-> Output:

|product_name|
| -----------|
| "iPhone 15 Pro"|
|"Samsung Galaxy Watch 4"|
|"Leather Case for iPhone 15 Pro"|
|"Wireless Charging Pad"|


and another without quotes:

|product_name|
| -----------|
|iPhone 15 Pro|
|Samsung Galaxy Watch 4|
|Leather Case for iPhone 15 Pro|
|Wireless Charging Pad|

2) Operator(#>/#>>) example

The both #> and #>> allows us to extract a JSONB by providing a PATH.
While the later will transform result to text.

```sql
SELECT data#>'{attributes,color}' FROM products;
SELECT data#>>'{attributes,color}' FROM products;
```
slight difference with 1 example is that this can extract JSONB much deeper with a simple operator.

3) Operator(@>/<@) example

The both operator return true if JSONB_former contains JSONB_later.

```sql
SELECT data->>'name' as name, data->>'price' as price 
FROM products WHERE data @> '{"price":999.99}';

SELECT data->>'name' as name, data->>'price' as price 
FROM products WHERE '{"price":999.99}' <@ data;
```

4) Operator(||) example

The operator || concatenates two JSONB values into a single one.

```sql
SELECT '{"name":"iphone177"}'::jsonb || '{"price":13332}'::jsonb as product;
```
|product|
| ----- |
|{"name":"iphone177","price":133332}|

5) Operator(?/?|/?&) example

The operator ? returns true if a key (after ?) exists as a top-level key of  jsonObject/json array (before ?), or false otherwise.

```sql
SELECT 
  id, 
  data ->> 'name' product_name, 
  data ->> 'price' price
FROM 
  products 
WHERE 
  data ? 'price';
```

The operator ?| returns true if a key from an array (after ?) exists as a top-level key of  jsonObject/json array (before ?) at least one, or false otherwise.

```sql
SELECT 
  id, 
  data ->> 'name' product_name, 
  data ->> 'price' price
FROM 
  products 
WHERE 
  data->'attributes'? array ['price'|'size'];
```

The operator ?& returns true if all the keys from an array (after ?) exists as a top-level key of  jsonObject/json array (before ?), or false otherwise.

```sql
SELECT 
  id, 
  data ->> 'name' product_name, 
  data ->> 'price' price
FROM 
  products 
WHERE 
  data->?& array ['color'|'storage'];
```

6) Operator(-) example

The operator - also allows you to delete all matching keys (with their values) from a JSON object or matching elements from a JSON array:

```sql
SELECT 
  '{"name": "John Doe", "age": 22}' :: jsonb - 'name' result;

SELECT 
  '{"name": "John Doe", "age": 22, "email": "john.doe@test.com"}' :: jsonb - array[ 'age', 
  'email' ] result;
SELECT 
  '["PostgreSQL", "API", "Web Dev"]' :: jsonb - ARRAY['API','Web Dev'] result;
```

7）Operator(@?) example

The Operator @? returns true if the condition is satisfied or the items exists. It has the function that both ? and @@ have

```sql
SELECT
  data ->> 'name' product_name 
FROM
  products 
WHERE
  data @? '$.price';

SELECT
  data ->> 'name' product_name 
FROM
  products 
WHERE
  data @? '$.price ? (@ > 999)';
```
8) Operator(@@) example

The Operator @? returns true if the condition is satisfied.

```sql
SELECT
  data ->> 'name' product_name 
FROM
  products 
WHERE
  data @@ '$.price>999'
```