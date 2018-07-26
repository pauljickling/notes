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

## Data Fields

Fields have several properties that define and constrain the type of data that they accept.

### Data Types

Fields in a SQL table will specify the data type expected for that field. Data types will vary depending on the specific SQL implementation, but common ones include strings, integers, date/time objects, and booleans.

### Nullable Fields

Fields are declared as either null or not null. If a field is not null, it must have a non-null entry, and this applies to the entire record.

### Field Constraints

Fields can be constrained with the declaration that they are unique. This means that there cannot be duplicate values for a field. An example of a unique field might be a social security number, there shouldn't be two instances of one. Other constraints can be defined to restrict data. For example, a phone number field might restrict to valid phone number sequences.

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

## Normalization

Normalization is a process that reduces redundant data in a database, and helps make the database more comprehensible to human beings. At a highest level, this involves separating the database into comprehensible tables. This comes at a performance cost as the computer has to spend extra effort looking up and joining tables to perform queries. When seeking greater database optimization, sometimes a denormalization process will be performed.
