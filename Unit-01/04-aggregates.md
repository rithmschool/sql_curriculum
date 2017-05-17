# Aggregates

### Objectives:

By the end of this chapter, you should be able to:

- Use built in aggregate functions to calculate data
- Use `GROUP BY` to aggregate data into sub groups
- Use `CASE` statements for custom conditional output

Very commonly, we will want to take multiple values in a table and group them into sub-categories or a single category based on an aggregate function. Let's look at a few aggregate functions, but first - some sample data:

```sql

CREATE TABLE sales (id SERIAL PRIMARY KEY, product TEXT, customer_name TEXT, price REAL, quantity SMALLINT);

INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Chair', 'Elie', 99.99, 1);
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Table', 'Tim', 250.00, 1);
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Chair', 'Matt', 49.99, 3);
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Table', 'Janey', 1000.00, 2);
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Chair', 'Janey', 300.00, 2);
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Table', 'Tim', 2200.00, 2);
INSERT INTO sales (product, customer_name, price, quantity) VALUES ('Bookshelf', 'Elie', 1200.00, 2);

```

You can read more about these data types [here](https://www.postgresql.org/docs/9.1/static/datatype-numeric.html).

Now that we have some sample data, let's examine a few common aggregate functions which collect multiple pieces of data and return a single value.

### COUNT

To count the number of occurances we use the `COUNT` function.

```sql
SELECT COUNT(*) FROM sales;
SELECT COUNT(*) FROM sales WHERE product = 'Chair';
```

### SUM

To figure out the sum we can use the `SUM` function and even round numbers using the `ROUND` function as well.

```sql
SELECT SUM(price) FROM sales;
SELECT ROUND(SUM(price)) FROM sales;
```

### MIN 

To find the minimum value in a data set we use the `MIN` function.

```sql
SELECT MIN(price) FROM sales;
```

### MAX

To find the maximum value in a data set we use the `MAX` function.

```sql
SELECT MAX(price) FROM sales;
```

### AVG

To find the average value in a data set we use the `AVG` function. We can attach the `AS` command to alias the column name.

```sql
SELECT AVG(price) AS max_count FROM sales;
```

### GROUP BY

Now that we have seen a couple aggregate functions, lets take some information.

```sql
SELECT product, COUNT(product) FROM sales GROUP BY product;
```

### HAVING

When using a `GROUP BY` clause, we can not attach a `WHERE` if we want to be more selective. Instead we use the `HAVING` keyword to place condition on our `GROUP BY` command.

```sql
SELECT product, COUNT(product) FROM sales GROUP BY product HAVING COUNT(product) > 2;
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
         WHEN price > 100 AND price < 400 THEN 'expensive'
         ELSE 'very expensive' END AS how_expensive
    FROM sales;
```

