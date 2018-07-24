# SQL

SQL stands for *Structured Query Language*, and it is the standard language used to communicate with relational databases. There are many flavors of SQL available (MySQL, PostgresSQL, SQLite, etc.) These notes will be referencing PostGresSQL.

## Relational Databases

A relational database is a database that contains database objects, the most common of which are tables. Each table contains fields, and rows of data. Every table will also have a row that contains a primary identification key, and different tables can form relationships by sharing these keys.

## Basics

Starting up SQL:

`sudo su postgres`

Creating a database:

`createdb {db name}`

Entering SQL shell:

`psql`

Exiting shell:

`\q`

Exiting SQL:

`exit`

## Data Types

Fields in a SQL table will specify the data type expected for that field. Data types will vary depending on the specific SQL implementation, but common ones include strings, integers, date/time objects, and booleans.

## Database Objects

In addition to tables, other kinds of database objects that might exist (but are not limited to) include views, clusters, sequences, indexes, and synonyms.

Database objects are organized by schemas. The NoSQL equivalent of a schema is the collection. Thus, the hierarchy looks something like this:

Database --> Schema --> Database Object --> Records (i.e. database object instances)

In SQL, schemas are associated with the user that created them. So in a database with multiple users, when you are querying a database object created by a different user you query `{USERNAME}.{DB_NAME}`.

## SQL Commands

Creating a new table:

`CREATE TABLE {table name}
( {field} {data type} {null or not null},
  {etc.});`

Altering a table:

`ALTER TABLE {table name} {params}`

Dropping a table:

`DROP TABLE {table name} {restrict or cascade}`
