# CRUD in SQL

### Objectives:

By the end of this chapter, you should be able to:

- Compare and contrast DML and DDL
- Use DDL to perform CRUD on tables, columns and databases
- Use DML to perform CRUD on records in a table

### DDL vs DML

When working with SQL, the commands you will be using fall into two major categories:

**DDL** - Data Definition Language - this refers to the SQL syntax and commands around creating, modifying and deleting **tables**, **columns** and **databases**.

**DML** - Data Manipulation Language - this refers to the SQL syntax and commands around creating, reading, modifying and deleting **rows**.

Let's first focus on DDL.

### Creating a table (DDL)

Open up postgres by typing `psql` in your terminal.  Next enter the following:

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY,
                    first_name TEXT,
                    last_name TEXT);
\d+ users
```

In the example above, `users` is the name of the table we are creating. The `id`, `first_name`, and `last_name` are all columns in the `users` table.  `SERIAL` and `TEXT` are examples of data types, which we will talk about in detail next. `PRIMARY KEY` is a constraint placed on the column.

So this example creates a `users` table with 3 columns: `id`, `first_name`, `last_name`.

### Data Types in Postgres

In relational databases like postgres, you must specify the type of data that you plan to store in a column.  Here are the types in postgres:

* **SERIAL** - auto incrementing integer, perfect for IDs
* **TEXT** - pieces of text (equally as space efficient as VARCHAR)
* **VARCHAR** - a variable number of characters
* **CHAR** - a fixed number of characters
* **BOOLEAN** - a boolean
* **INTEGER** - an integer
* **REAL** - a floating point number, e.g., 3.141593
* **DECIMAL**, **NUMERIC** - floating point numbers that have user specified percision.
* **MONEY** - floating point numbers use for money
* **ARRAY** - an array (array types are rarely used)

### Constraints

So what is `PRIMARY KEY`? A primary key enforces the constraint of "uniqueness". If we made a user with an id of `1` and then tried to add another user with an id of `1` explicitly, SQL will not allow us to do that, because of our primary key. Placing primary keys on columns like `id` are commonplace. 

### Adding a column in a table

```sql
ALTER TABLE users ADD COLUMN favorite_number INTEGER; 
\d+ users -- we should see our favorite number here
```

### Removing a column in a table

```sql
ALTER TABLE users DROP COLUMN favorite_number; 
\d+ users -- we should see our favorite number not here
```

### Renaming columns in a table

```sql
ALTER TABLE users ADD COLUMN jobb TEXT; 
ALTER TABLE users RENAME COLUMN jobb TO job; 
\d+ users -- we should see our column spelled correctly here
```

### Changing the datatype of a column in a table

```sql
ALTER TABLE users ADD COLUMN favorite_number TEXT; 
ALTER TABLE users ALTER COLUMN favorite_number SET DATA TYPE VARCHAR(100); 
\d+ users -- we should see our column spelled correctly here
```

### Creating and Modifying data (DML)

When we perform CRUD operations on our rows (not columns, tables or databases) we are using DML or Data Manipulation Language. 

| CRUD  | SQL  |
|---|---|
| Create  | INSERT  |
| Read  | SELECT  |
| Update  |  UPDATE  |
| Delete  |  DELETE  |

Let's get started with reading data from our tables using `SELECT`.

### SELECT

```sql
--to select all rows and columns--
SELECT * FROM users;

--to select specific columns--
SELECT id, first_name FROM users;

--to select specific columns and rows--
SELECT id, first_name FROM users WHERE id=1;
```

### INSERT

To insert or add data to a table - we use the INSERT command

```sql
--start with the INSERT INTO commands and then specify a table(column1, column2, ...) and VALUES for each column.
INSERT INTO users(first_name, last_name) VALUES ('Elie', 'Schoppik');
```

### UPDATE

To update a row or multiple rows we use the `DELETE UPDATE` command.

```sql
UPDATE users SET first_name = 'Elie'; -- will update all users
UPDATE users SET first_name = 'Elie' WHERE id=1; -- will update all users
```

### DELETE

To delete a row or multiple rows we use the `DELETE FROM` command.

```sql
DELETE FROM users; -- will delete all users
DELETE FROM users WHERE id=1; -- will delete all users
```


