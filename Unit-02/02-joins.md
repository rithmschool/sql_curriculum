# Joins

###Objectives

By the end of this chapter, you should be able to:

* Describe what a join between two tables does
* Describe the difference between inner joins, outer joins and cross joins
* Create a query with multiple joins

### What is a join?

A query with a join combines data from two different tables.  Typically, the join combines data based on a matching row.

For example, if we have a table like the following:

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


If we were to do a join on the `id` column from the `people` table with the `people_id` column from the `interests` table, our results would be:

__people__ joined with __interests__ on `people.id` = `interests.people_id`:

|people.id|first_name|last_name|interests.id|interest|people_id|
| ----    |---       | ---     |---         |---     |---      |
| 1 | Billy Jean| King| 2 | tennis | 1 |
| 2 | Dawn | Riley | 1 | sailing| 2 |
| 3 | Grace | Hopper | 3 | software | 3 |
| 3 | Grace | Hopper | 4 | debugging | 3 |

Notice that we get two rows with the person Grace Hopper because in this data set, Grace Hopper has two matches in the `interests` table, therefore, the match is represented with two different rows in the resulting join.


### Joins in SQL

Let's start by creating a database and filling it with our sample data.  Make sure to delete the database and recreate it if you have made it already. It is important that the `id`s from people start counting from 1.

```sql
CREATE TABLE people (id SERIAL PRIMARY KEY,
                    first_name TEXT,
                    last_name TEXT);

CREATE TABLE interests (id SERIAL PRIMARY KEY,
                       interest TEXT,
                       people_id INTEGER REFERENCES people);

INSERT INTO people (first_name, last_name) VALUES ('Billy Jean', 'King'),
                                                  ('Dawn', 'Riley'),
                                                  ('Grace', 'Hopper');

INSERT INTO interests (interest, people_id) VALUES ('sailing', 2),
                                                   ('tennis', 1),
                                                   ('software', 3),
                                                   ('debugging', 3);
```


Now that we have some data, let's do our first join:

```sql
SELECT *
FROM people
INNER JOIN interests ON people.id=interests.people_id;
```

Your output for the query should be the same as our example tables from above. So what are we doing?  We are combining all rows from people with all possible rows from interests where the `people.id` is equals to the `interests.people_id`.  In our example, the `INNER` part of our join is not necessary.  You could also write the join this way:

```sql
SELECT *
FROM people
JOIN interests ON people.id=interests.people_id;
```
Even thought the `INNER` keyword is not necessary, it helps us remember what kind of join we are doing.  Next we'll see other types of joins

### Left, Right, Full (Outer) Join

So far we have looked a data set where all data from the first table has at least 1 match in the second table, and vice versa.  Sometimes that is not the case though.  Consider the following data set (if you are following along, you should drop the previous tables and create a new ones):


```sql
CREATE TABLE people (id SERIAL PRIMARY KEY,
                    first_name TEXT,
                    last_name TEXT);

CREATE TABLE interests (id SERIAL PRIMARY KEY,
                       interest TEXT,
                       people_id INTEGER REFERENCES people);

INSERT INTO people (first_name, last_name) VALUES ('Billy Jean', 'King'),
                                                  ('Dawn', 'Riley'),
                                                  ('Grace', 'Hopper'),
                                                  ('Moxie', 'Garcia');

INSERT INTO interests (interest, people_id) VALUES ('sailing', 2),
                                                   ('tennis', 1),
                                                   ('software', 3),
                                                   ('debugging', 3),
                                                   ('snow boarding',null),
                                                   ('ham radio',null);
```

In this data set, Moxie Garcia does not have any interests.  Also, none of the people are interested in snow boarding or ham radios.

When we do an inner join, the results are only rows where a person matches an interest. But sometimes we may want to include people without interests, or only find people with no interests.  We can solve this problem with left or right joins.

Here is how we would answer the following questions:

* __Find all people and all of their interests (including people with no interests):__

```sql
SELECT *
FROM people
LEFT JOIN interests ON people.id=interests.people_id;
```

Here are the results:

| id | first_name | last_name | id | interest  | people_id |
|----|------------|-----------|----|-----------|-----------|
|  2 | Dawn       | Riley     |  1 | sailing   |         2|
|  1 | Billy Jean | King      |  2 | tennis    |         1|
|  3 | Grace      | Hopper    |  3 | software  |         3|
|  3 | Grace      | Hopper    |  4 | debugging |         3|
|  4 | Moxie      | Garcia    |    |           |          |


The `LEFT JOIN` gets us all the rows from the left (the left in this case is the `people` table) that have a match in the interests table as well as all the people that do not have a match.

We could get the same results by doing a right join. In this case, a right join gets all the matches from the right (the people table) plus all of the people without any interest matches.

```sql
SELECT *
FROM interests
RIGHT JOIN people ON people.id=interests.people_id;
```

We can also add the keyword `OUTER` to remind ourselves that this is an outer join.  Just like `INNER`, this is an optional keyword that you can add, but it doesn't change the query:

```sql
SELECT *
FROM interests
RIGHT OUTER JOIN people ON people.id=interests.people_id;
```


* __Find all of the people that have no interests:__

```sql
SELECT *
FROM people
LEFT JOIN interests ON people.id=interests.people_id
WHERE interests.id IS NULL;
```
In this query, we first get all of the people and all of their interests (including people with no interests), then we limit that result to only the rows without an interest id.  In other words, the results where people do not have a matching interest.  Your output should be:

| id | first_name | last_name | id | interest | people_id |
|----+------------+-----------+----+----------+-----------|
|  4 | Moxie      | Garcia    |    |          |          |

* __Find all people and all interests (even if there is no corresponding match)__:

```sql
SELECT *
FROM people
FULL JOIN interests ON people.id=interests.people_id;
```
Here are the results:

| id | first_name | last_name | id |   interest    | people_id |
|----+------------+-----------+----+---------------+-----------|
|  2 | Dawn       | Riley     |  1 | sailing       |         2|
| 1 | Billy Jean | King      |  2 | tennis        |         1|
|  3 | Grace      | Hopper    |  3 | software      |         3|
|  3 | Grace      | Hopper    |  4 | debugging     |         3|
|    |            |           |  5 | snow boarding |          |
|   |            |           |  6 | ham radio     |          |
|  4 | Moxie      | Garcia    |    |               |          |

So with our `FULL JOIN` we are getting all matching combinations of people and interests, plus interests that do not have any matching people and people that do not have any matching interests.

### Join Diagram

Here is a useful diagram from [C.L. Moffet's article](http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins) that illustrates the common joins which we just covered:

![Ven Diagram of Joins](http://www.codeproject.com/KB/database/Visual_SQL_Joins/Visual_SQL_JOINS_orig.jpg)

### Cross Join

A cross join is not the most useful join, but it may come in handy once and a while.  It gives you all possible combinations of rows from the first table and rows from the second table.  Here is the query:

```sql
SELECT *
FROM people
CROSS JOIN interests;
```

And this is the output:

| id | first_name | last_name | id |   interest    | people_id |
|----|------------|-----------|----|---------------|-----------|
|  1 | Billy Jean | King      |  1 | sailing       |         2|
|  2 | Dawn       | Riley     |  1 | sailing       |         2|
|  3 | Grace      | Hopper    |  1 | sailing       |         2|
|  4 | Moxie      | Garcia    |  1 | sailing       |         2|
|  1 | Billy Jean | King      |  2 | tennis        |         1|
|  2 | Dawn       | Riley     |  2 | tennis        |         1|
|  3 | Grace      | Hopper    |  2 | tennis        |         1|
|  4 | Moxie      | Garcia    |  2 | tennis        |         1|
|  1 | Billy Jean | King      |  3 | software      |         3|
|  2 | Dawn       | Riley     |  3 | software      |         3|
|  3 | Grace      | Hopper    |  3 | software      |         3|
|  4 | Moxie      | Garcia    |  3 | software      |         3|
|  1 | Billy Jean | King      |  4 | debugging     |         3|
|  2 | Dawn       | Riley     |  4 | debugging     |         3|
|  3 | Grace      | Hopper    |  4 | debugging     |         3|
|  4 | Moxie      | Garcia    |  4 | debugging     |         3|
|  1 | Billy Jean | King      |  5 | snow boarding |          |
|  2 | Dawn       | Riley     |  5 | snow boarding |          |
|  3 | Grace      | Hopper    |  5 | snow boarding |          |
|  4 | Moxie      | Garcia    |  5 | snow boarding |          |
|  1 | Billy Jean | King      |  6 | ham radio     |          |
|  2 | Dawn       | Riley     |  6 | ham radio     |          |
|  3 | Grace      | Hopper    |  6 | ham radio     |          |
|  4 | Moxie      | Garcia    |  6 | ham radio     |          |


### Self Join

A self join is actually just doing a normal join.  The big difference is that the two tables that you are joining are the same.  The classic example is an employee table with an id for a boss.  The `boss_id` in the table references another employee in the same table:

__employees__

 id | first_name | last_name | boss_id |
----+------------+-----------+----+
  1 | Billy Jean | King      |  3 |
  2 | Dawn       | Riley     |  3 |
  3 | Grace      | Hopper    |  4 |
  4 | Ada        | Lovelace  |    |
  5 | Emmy       | Noether   |  4  |
  6 | Marie      | Curie     |  5  |

Here is the sql for the table:

```sql
CREATE table employees (id SERIAL PRIMARY KEY,
                        first_name TEXT,
                        last_name TEXT,
                        boss_id INTEGER);

INSERT INTO employees (first_name, last_name, boss_id) VALUES ('Billy Jean', 'King', 3),
                                                  ('Dawn', 'Riley', 3),
                                                  ('Grace', 'Hopper', 4),
                                                  ('Ada', 'Lovelace', NULL),
                                                  ('Emmy', 'Noether', 4),
                                                  ('Marie', 'Curie', 5);
```
* __Find all the employees and their bosses. Also show employees without bosses__:



```sql
SELECT e.id, e.first_name, e.last_name, e.boss_id, b.first_name as b_first_name, b.last_name as b_last_name
FROM employees e
LEFT JOIN employees b ON e.boss_id=b.id;
```
