# Aggregates

Very commonly, we will want to take multiple values in a table and group them into sub-categories or a single category based on an aggregate function. Let's look at a few aggregate functions, but first - some sample data:

```sql

CREATE TABLE sales (id SERIAL PRIMARY KEY, product TEXT, customer_name TEXT, price REAL, quantity, SMALLINT)

INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Chair', 'Elie', 99.99, 1)
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Table', 'Tim', 250.00, 1)
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Chair', 'Matt', 49.99, 3)
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Table', 'Janey', 1000.00, 2)
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Chair', 'Janey', 300.00, 2)
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Table', 'Tim', 2200.00, 2)
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Bookshelf', 'Elie', 1200.00, 2)

```

You can read more about these data types [here](https://www.postgresql.org/docs/9.1/static/datatype-numeric.html).

### COUNT

```sql
SELECT COUNT(*) FROM sales;
```

### SUM

```sql
SELECT SUM(price) FROM sales;
```

### MIN 

```sql
SELECT MIN(price) FROM sales;
```

### MAX

```sql
SELECT MAX(price) FROM sales;
```

### AVG

```sql
SELECT AVG(price) FROM sales;
```

### GROUP BY

Now that we have seen a couple aggregate functions, lets take some information.

```sql
SELECT price, SUM(*) AS total
    FROM sales
    GROUP BY price;
```

```sql
SELECT price,quantity, SUM(*) AS total
    FROM sales
    GROUP BY price,quantity;
```

### HAVING

```sql
SELECT price, SUM(*) AS total
    FROM sales
    GROUP BY price
    HAVING price > 100;
```

### DISTINCT

If we only want to find unique values in a column, we can use `DISTINCT`, we can also do this for pairs of columns separated by a comma.

```sql
SELECT DISTINCT customer_name FROM sales;
```

### CASE 

In SQL, we can use conditional logic to query our data and display custom results based off of the condition.

```sql
SELECT product, price, 
    CASE WHEN price < 50 THEN 'inexpensive'
         WHEN price > 50 AND price < 100 THEN 'reasonable'
         WHEN price < 50 AND price < 400 THEN 'expensive'
         ELSE 'very expensive' END AS how_expensive
    FROM sales;
```




