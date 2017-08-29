# SQL Associations

### Objectives

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

In our sample data, the values in the `people_id` column reference the `id` from the people table.  Take the row from the `people` table with the `id` of 3 as an example.  We can see that id 3, Grace Hopper, is associated with 2 rows in the interests table because the `people_id` matches the id of 3.  The two interests are software and debugging.

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

### One to Many

In our foreign key example above, we actually setup a one to many (1:M) association. The way we structured the database implies that one person can have many interests, and that each interest belongs to exactly one person.

One to many associations are extremely common in database and the association usually implies that one row from a table owns or has rows from another table.

For example:

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY,
                   email TEXT,
                   password TEXT);

CREATE TABLE photos (id SERIAL PRIMARY KEY,
                     url TEXT,
                     user_id INTEGER REFERENCES users);
```

In the table above, each user in the users table, can have many photos and one photo from the photos table belongs to exactly one user based on the value of `user_id`.

Another example:

```sql
CREATE TABLE states (id SERIAL PRIMARY KEY,
                    name TEXT);

CREATE TABLE cities (id SERIAL PRIMARY KEY,
                     name TEXT,
                     state_id INTEGER REFERENCES states);
```

In this example, a state has many cities and a city belongs to a state. So this is a one to many relationship as well.


### Many to Many

Often your database needs to model a more complex association in which an item from one table can associate to many rows in another table and vice versa.  This is a many to many (M:M) relationship.  

Some examples:

* A database with orders and products.  A product can have many orders its associated with and an order has many products
* School courses and students.  A student can be enrolled in many classes and a class has many students.
* Recipes and ingredients.  A recipe can have many ingredients and ingredients can be used in many recipes.

__Many to Many in SQL__

Let's create an association between students and courses in sql.  In order to make this association work, we will need a third table that maps a foreign key from courses to a foreign key from students.  We will call that third table enrollments in this case:

```sql
CREATE TABLE students (id SERIAL PRIMARY KEY,
                       name TEXT);

CREATE TABLE courses (id SERIAL PRIMARY KEY,
                      name TEXT,
                      teacher_name TEXT);

CREATE TABLE enrollments (id SERIAL PRIMARY KEY,
                      	   student_id INTEGER REFERENCES students,
                      	   course_id INTEGER REFERENCES courses);
```

In students and courses example, one student can be associated with a course by a single row in the `enrollments` table. If that student is in another course, there will be another row in the `enrollments` table that associates the same student id to another course id.

Let's look at some sample data for the example:

__students__

|id|first_name|last_name
| ----|---|---|
| 1 | Billy Jean| King|
| 2 | Dawn | Riley |
| 3 | Grace | Hopper |

__courses__

|id|name|teacher_name|
| ----|---|---|
| 1 | Physics| Mrs. Skiles |
| 2 | P.E. | Mr. Carty|
| 3 | Biology | Mrs. Waldorf|
| 4 | Computer Science | Miss. Flurry |

__enrollments__

|id|student_id|course_id|
| ----|:---:|:---:|
| 1 | 1 | 2|
| 2 | 2 | 2|
| 3 | 3 | 4|
| 4 | 3 | 1|
| 5 | 3 | 3|
| 6 | 1 | 3|

So based on this data, Billy Jean King is taking P.E. and Biology, Dawn Riley is only taking P.E. and Grace Hopper is taking Computer Science, Biology, and Physics.

The same foreign key constraints apply to the enrollments table.  If a `student_id` does not exist in the `students` table, then the data will not be allowed to be inserted into the `enrollments` table.  Likewise, if a `course_id` does not exist in the `courses` table, the data will not be allowed to be inserted into the `enrollments` table.
