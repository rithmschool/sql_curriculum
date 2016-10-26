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

### SELECT

```sql
SELECT * FROM users;
```

### INSERT

```sql
INSERT INTO users(first_name, last_name) VALUES ('Elie', 'Schoppik');
```

### UPDATE

```sql
UPDATE users SET first_name = 'Elie' -- will update all users
UPDATE users SET first_name = 'Elie' WHERE id=1 -- will update all users
```

### DELETE

```sql
DELETE FROM users; -- will delete all users
DELETE FROM users WHERE id=1; -- will delete all users
```


