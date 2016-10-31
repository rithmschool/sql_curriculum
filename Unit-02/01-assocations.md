# SQL Associations

### Objectives:

By the end of this chapter, you should be able to:

- Define __foreign key__
- Create a one to many (1:M) association in SQL
- Create a many to many (M:M) association in SQL

### Associations Intro

So far, all of our sql table examples have used a single table.  However, one of the most important features of relational databases is the ability to associate one table to another.  The first thing we need to understand in order to create these associations of data is a __foreign key constraint__.


### Foreign Key

A foreign key is a constraint placed on a column.  It associates the data in one column to a column in a different table.  Let's look at an example:

```sql
CREATE TABLE people (id SERIAL PRIMARY KEY,
                    first_name TEXT,
                    last_name TEXT);

CREATE TABLE interests (id SERIAL PRIMARY KEY,
                       interest TEXT,
                       people_id INTEGER REFERENCES people);
```

In our table definition, we have a people table and we have an interests table.  Using the foreign key constraint in the interest table we have associated the `people_id` column with the `id` column from the `people` table.

Let's look at the two tables visually with some sample data:

__people__

|id|first_name|last_name|
| ----|---|---|
| 1 | Billy Jean| King|
| 2 | Dawn | Riley |
| 3 | Grace | Hopper |

__interests__

|id|interest|people_id|
| ----|---|---|
| 1 | sailing| 2 |
| 2 | tennis | 1 |
| 3 | software | 3 |
| 4 | debugging | 3 |

In our sample data, the values in the `people_id` column reference the `id` from the people table.  Take the row form the `people` table with the `id` of 3 as an example.  We can see that id 3, Grace Hopper, is associated with 2 rows in the interests table because the `people_id` matches the id of 3.  The two interests are software and debugging.

But why is a foreign key considered a constraint?  Well the database actually ensures that the data in your foreign key column exists in the table that the foreign key references.  Try inserting into the interests table with data that does not exist in the people table:

```sql
INSERT INTO interests (interest, people_id) 
            VALUES ('scuba diving', 30);
```

After you run the insert statement, you should see an error like this:

```
ERROR:  insert or update on table "interests" violates 
        foreign key constraint
```


### One to One

A common example of a 1:1 is a person and a social security number. 

### One to Many

Some common examples of 1:M

### Many to Many

A common example of a M:M

### Joins

Let's start with some sample data

```sql
CREATE TABLE 
```

### Inner Join

### Outer Join

### Cross Join

### Self Join

### Union

### Union All

### Intersect

### Except