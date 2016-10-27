# CRUD in SQL

### Objectives:

By the end of this chapter, you should be able to:

- Compare and contrast DML and DDL
- Use DDL to perform CRUD on tables, columns and databases
- Use DML to perform CRUD on records in a table

### DDL vs DML

When working with SQL, it's essential to compare and contrast two terms, DDL and DML.

**DDL** - Data Definition Language - this refers to the SQL syntax and commands around creating, modifying and deleting **tables**, **columns** and **databases**.

**DDL** - Data Manipulation Language - this refers to the SQL syntax and commands around creating, reading, modifying and deleting **rows**.

Let's first focus on DDL.

### Creating a table (DDL)

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, first_name TEXT, last_name TEXT);
\d+ users
```

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
ALTER TABLE users ALTER COLUMN favorite_number SET DATA TYPE INTEGER; 
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

There are two different ways of inserting or adding data to a table - let's take a look at both

```sql
--start with the INSERT INTO commands and then specify a table(column1, column2, ...) and VALUES for each column.
INSERT INTO users(first_name, last_name) VALUES ('Elie', 'Schoppik');

--if you want to add sequentially to columns you do not need to specify the first portion-- 
INSERT INTO users(first_name, last_name) VALUES ('Elie', 'Schoppik');
```

### UPDATE

To update a row or multiple rows we use the `DELETE UPDATE` command.

```sql
UPDATE users SET first_name = 'Elie' -- will update all users
UPDATE users SET first_name = 'Elie' WHERE id=1 -- will update all users
```

### DELETE

To delete a row or multiple rows we use the `DELETE FROM` command.

```sql
DELETE FROM users; -- will delete all users
DELETE FROM users WHERE id=1; -- will delete all users
```


