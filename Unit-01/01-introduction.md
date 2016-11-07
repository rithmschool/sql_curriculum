# Introduction

### Objectives:

By the end of this chapter, you should be able to:

- Define what a relational database and SQL are
- Have Postgres installed on your machine
- Use basic psql syntax to list essential information about tables and databases

Welcome to the SQL curriculum! We will be learning how to use SQL to communicate with a database and store information permanently.  Let's get started by learning a few essential definitions.

### Definitions

**Database** - a collection of information that can easily be updated, accessed and managed. Databases are used to capture and analyze data on a more permanent bases.

**Relatonal Database** - a type of database that is structured so that relationships can be established among stored information. 

**SQL** - Structured Query Language is the standardized language for communicating with and managing data in relational database. The acronym is pronounced like the word "sequel".

**RDBMS** - A relational database management system is a database management system based on a "relational" model. This model was actually developed before the SQL language and is the basis for SQL and systems like MySQL, Postgres, Oracle, IBM DB2, Microsoft SQL Server and many more.

**PostgreSQL** - PostgresQL or "postgres" is a relational database management system that is open source (free for everyone to use and contribute too!). Postgres powers some of the largest companies in the world.

**Schema** - the organization of data inside of a database. The database schema represents the collection and association of tables in a database.

**Table** - A series of columns and rows which store data inside of a database. An example of a table is "users" or "customers".

**Column** - A portion of a table which has a specific category and data type. If we had a table called "users", we could create columns for "username", "password", which would both be a variable amount of characters or text. Postgres has quite a few data types, which we will see later

**Row / Record** - Each row in a table represents a record stored. In our "users" table, we may have a row that looks like 1, "elie", "secret". Where 1 represents a unique id, "elie" represents the value of the "username" and "secret" represents the value of the "password".

**psql** - a command line program, which can be used to enter SQL queries directly, or executed from a file.

### Installing Postgres

To install postgres, follow these steps:

```sh
brew install postgres --no-ossp-uuid
brew pin postgres

# Initialize db if none exists already
initdb /usr/local/var/postgres

# Create launchctl script
mkdir -p ~/Library/LaunchAgents
cp /usr/local/Cellar/postgresql/VERSION/homebrew.mxcl.postgresql.plist ~/Library/LaunchAgents/

# Edit launchctl script (set to not start automatically and keepalive false)
subl ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

# Inject launchctl script
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

# Start PostgreSQL
start pg
```

You should be able to type in `psql` and see a new shell open up. To quit out of `psql` type in `\q`.

### PSQL Syntax

Let's examine some useful postgres commands you will be using in `psql`:

`\du` - lists users
`\dt` - lists tables
`\d+ table_name` - list details about the table name
`\l` - lists databases
`\c NAME_OF_DB` - connect to a database

If you would like to create a new database, we can do this in 2 ways:

1. In `psql` type `CREATE DATABASE name_of_db;`
2. In the `terminal` type `createdb name_of_db`

If you would like to remove a database, we can do this in 2 ways:

1. In `psql` type `DROP DATABASE name_of_db;` - make sure you are not connected to that database or the command will not work
2. In the `terminal` type `drop name_of_db`

### SQL Syntax

The most important thing with SQL syntax is to end your statements with a `;`. SQL will not understand when you have finished your statement unless it sees that. You also MUST make sure to put all text in **single** quotes not double quotes. SQL views double quotes as a name of a table and single quotes as a string.







