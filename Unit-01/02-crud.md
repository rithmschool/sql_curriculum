# CRUD in SQL

### DDL vs DML

When working with SQL, it's essential to 

### Modifying a table (DDL)

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, first_name TEXT, last_name TEXT)l
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
UPDATE users SET first_name = 'Elie' # will update all users
UPDATE users SET first_name = 'Elie' WHERE id=1 # will update all users
```

### DELETE

```sql
DELETE FROM users; # will delete all users
DELETE FROM users WHERE id=1; # will delete all users
```


