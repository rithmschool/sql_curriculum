# Operators

### Objectives:

By the end of this chapter, you should be able to:

- Use built in operators to write more complex queries
- Use `ORDER BY` to categorize results

Let's start with the following SQL

```sql

psql

DROP DATABASE IF EXISTS sports; -- in case you copy this whole set of commands multple times
CREATE DATABASE sports;

\c sports

CREATE TABLE players (id SERIAL PRIMARY KEY, name TEXT, sport TEXT, team TEXT, jersey_number INTEGER, is_rookie BOOLEAN);

INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('Martin', 'hockey', 'devils', 12, false);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('David', 'baseball', 'mets', 2, true);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('David', 'soccer', 'galaxy', 17, false);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('Elie', 'baseball', 'cobras', 8, false);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('Lisa', 'basketball', 'sparks', 44, false);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('Seabass', 'frisbee', 'jumbos', 44, false);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('Sue', 'basketball', 'lynx', 54, false);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('Candace', 'sparks', 'cobras', 49, false);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('Swin', 'basketball', 'shock', 1, true);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('Gilbert', 'basketball', 'warriors', 0, false);
INSERT INTO players (name, sport, team, jersey_number, is_rookie) VALUES ('Maria', 'tennis', 'none', 24, false);
```

### WHERE

The building block for these operators is the `WHERE` clause which comes after any operation like `SELECT`, `UPDATE` or `DELETE`

### = 

```sql
SELECT * FROM players WHERE sport = 'basketball';

/*
 id |  name   |   sport    |   team   | jersey_number | is_rookie 
----+---------+------------+----------+---------------+-----------
  5 | Lisa    | basketball | sparks   |            44 | f
  7 | Sue     | basketball | lynx     |            54 | f
  9 | Swin    | basketball | shock    |             1 | t
 10 | Gilbert | basketball | warriors |             0 | f
*/
```

### IN

To find multiple records we can search for multiple terms using `IN`

```sql
SELECT * FROM players WHERE jersey_number IN (0,1);
/*
 id |  name   |   sport    |   team   | jersey_number | is_rookie 
----+---------+------------+----------+---------------+-----------
  9 | Swin    | basketball | shock    |             1 | t
 10 | Gilbert | basketball | warriors |             0 | f

*/
```

### NOT IN

We can do the exact inverse of `IN` using `NOT IN`

```sql
SELECT * FROM players WHERE jersey_number NOT IN (0,1);
/*
 id |  name   |   sport    |  team  | jersey_number | is_rookie 
----+---------+------------+--------+---------------+-----------
  1 | Martin  | hockey     | devils |            12 | f
  2 | David   | baseball   | mets   |             2 | t
  3 | David   | soccer     | galaxy |            17 | f
  4 | Elie    | baseball   | cobras |             8 | f
  5 | Lisa    | basketball | sparks |            44 | f
  6 | Seabass | frisbee    | jumbos |            44 | f
  7 | Sue     | basketball | lynx   |            54 | f
  8 | Candace | sparks     | cobras |            49 | f
 11 | Maria   | tennis     | none   |            24 | f

*/
```

### BETWEEN

To search for a field in a range, we can use `BETWEEN x AND y`.

```sql
SELECT * FROM players WHERE jersey_number BETWEEN 0 AND 25;
/*
 id |  name   |   sport    |   team   | jersey_number | is_rookie 
----+---------+------------+----------+---------------+-----------
  1 | Martin  | hockey     | devils   |            12 | f
  2 | David   | baseball   | mets     |             2 | t
  3 | David   | soccer     | galaxy   |            17 | f
  4 | Elie    | baseball   | cobras   |             8 | f
  9 | Swin    | basketball | shock    |             1 | t
 10 | Gilbert | basketball | warriors |             0 | f
 11 | Maria   | tennis     | none     |            24 | f

*/
```

### Arithmetic

SQL supports all kinds of arithmetic operators like `<`, `<=`, `>=`, `>` and `!=`.

```sql
SELECT * FROM players WHERE jersey_number > 25;
/*
 id |  name   |   sport    |  team  | jersey_number | is_rookie 
----+---------+------------+--------+---------------+-----------
  5 | Lisa    | basketball | sparks |            44 | f
  6 | Seabass | frisbee    | jumbos |            44 | f
  7 | Sue     | basketball | lynx   |            54 | f
  8 | Candace | sparks     | cobras |            49 | f

*/
```

### AND

To check that multiple conditions are satisfied we can use the `AND` command.

```sql
SELECT * FROM players WHERE jersey_number > 25 and id < 6;
/*
 id | name |   sport    |  team  | jersey_number | is_rookie 
----+------+------------+--------+---------------+-----------
  5 | Lisa | basketball | sparks |            44 | f

*/
```

### OR

To check that at least one condition is satisfied we can use the `OR` command.

```sql
SELECT * FROM players WHERE jersey_number > 25 or id < 6;
/*
 id |  name   |   sport    |  team  | jersey_number | is_rookie 
----+---------+------------+--------+---------------+-----------
  1 | Martin  | hockey     | devils |            12 | f
  2 | David   | baseball   | mets   |             2 | t
  3 | David   | soccer     | galaxy |            17 | f
  4 | Elie    | baseball   | cobras |             8 | f
  5 | Lisa    | basketball | sparks |            44 | f
  6 | Seabass | frisbee    | jumbos |            44 | f
  7 | Sue     | basketball | lynx   |            54 | f
  8 | Candace | sparks     | cobras |            49 | f

*/
```

### LIKE

To search for a term we can use the `LIKE` command. The `%` denotes any possible character. `LIKE` **is** case sensitive.

```sql
--find all players whose name starts with a capital S--
SELECT * FROM players WHERE name LIKE 'S%';
/*
 id |  name   |   sport    |  team  | jersey_number | is_rookie 
----+---------+------------+--------+---------------+-----------
  6 | Seabass | frisbee    | jumbos |            44 | f
  7 | Sue     | basketball | lynx   |            54 | f
  9 | Swin    | basketball | shock  |             1 | t
*/
```

### ILIKE

`ILIKE` is similar to the `LIKE` command, but it is **case insensitive**.

```sql
SELECT * FROM players WHERE name ILIKE 's%';
/*
 id |  name   |   sport    |  team  | jersey_number | is_rookie 
----+---------+------------+--------+---------------+-----------
  6 | Seabass | frisbee    | jumbos |            44 | f
  7 | Sue     | basketball | lynx   |            54 | f
  9 | Swin    | basketball | shock  |             1 | t
*/
```

### ORDER BY

If we want to order results in ascending or descending order we use the `ORDER BY ASC_or_DESC` command. 

```sql
SELECT * FROM players ORDER BY jersey_number DESC;
/*
 id |  name   |   sport    |   team   | jersey_number | is_rookie 
----+---------+------------+----------+---------------+-----------
  7 | Sue     | basketball | lynx     |            54 | f
  8 | Candace | sparks     | cobras   |            49 | f
  5 | Lisa    | basketball | sparks   |            44 | f
  6 | Seabass | frisbee    | jumbos   |            44 | f
 11 | Maria   | tennis     | none     |            24 | f
  3 | David   | soccer     | galaxy   |            17 | f
  1 | Martin  | hockey     | devils   |            12 | f
  4 | Elie    | baseball   | cobras   |             8 | f
  2 | David   | baseball   | mets     |             2 | t
  9 | Swin    | basketball | shock    |             1 | t
 10 | Gilbert | basketball | warriors |             0 | f
*/
```

### Changing Data Types

Commonly in SQL, we will want to output a certain data type for an operation, to convert one data type to another we can use the `CAST` command or use `::data_type`

```sql
SELECT round((SUM(id) / COUNT(jersey_number))::numeric ,2)::float 
```


