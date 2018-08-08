# SQL

SQL stands for *Structured Query Language*, and it is the standard language used to communicate with relational databases. There are many flavors of SQL available (MySQL, PostgreSQL, SQLite, etc.) These notes will be referencing PostGreSQL.

## Relational Databases

A relational database is a database that contains database objects, the most common of which are tables. Each table contains fields, and rows of data. Every table will also have a row that contains a primary identification key, and different tables can form relationships by sharing these keys.

## Basics

Starting up SQL before defining a user with createdb access:

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

```
CREATE TABLE {table name}
( {field} {data type} {null or not null},
  {etc.});
```

Altering a table:

`ALTER TABLE {table name} {params};`

Dropping a table:

`DROP TABLE {table name} {restrict or cascade};`

## Normalization

Normalization is a process that reduces redundant data in a database, and helps make the database more comprehensible to human beings. At a highest level, this involves separating the database into comprehensible tables. This comes at a performance cost as the computer has to spend extra effort looking up and joining tables to perform queries. When seeking greater database optimization, sometimes a denormalization process will be performed.

## Manipulating Data

Use insert operations to add a record.

```
INSERT INTO {table name}
VALUES ('value1', 'value2', [null]);
```

Insert operations need to include every field.

Use single character quotation marks for values. They are not required for integer and null values, but it is best practice to include them anyway for the sake of consistency.

SELECT is the keyword for querying data. By using the SELECT and INSERT commands together it is possible to insert data from one table into another.

```
INSERT INTO {table name} {field params}
SELECT {params}
FROM {table name}
{where conditions};
```

The where conditions help narrow the scope of the field queries. For example, among a last name field you might limit it to only records that contain the last name 'Chavez'.

It is important to make sure the queried fields have matching data types with the insert fields.

Use UPDATE to update existing entries.

```
UPDATE {table name}
SET {field name} = {value}
{where condition};
```

You can set multiple fields, and use commas after the values to separate them (the set keyword should only be used once, its like combining multiple variable assignment in a single line).

Use DELETE to remove all the field values for a record. The syntax is identical to UPDATE.

## Managing Database Transactions

A *transaction* is a unit of work performed on a database. In SQL these are accomplished via the INSERT, UPDATE, and DELETE operations. A transaction could be one of these operations, or it could be several of them strung together, however if one fails the entire unit fails.

SQL databases feature *transaction control* operations. These are COMMIT, ROLLBACK, and SAVEPOINT. Most flavors of SQL use auto-commits by default, so to use these operations you need to run the following:

`SET IMPLICIT_TRANSACTIONS ON;`

Anyone familiar with git should be familiar with the COMMIT and ROLLBACK operations. COMMIT is used to store changes made to the database that until they are committed, have only been changed in a temporary buffer. ROLLBACK undoes any changes to the previous commit.

A savepoint is used to create an intermediary save point before a commit. The syntax is:

`SAVEPOINT {savepoint name}`

Note that a savepoint name needs to be unique. Once this is created you can rollback to that savepoint:

`ROLLBACK TO {savepoint name}`

To remove a savepoint use the following command:

`RELEASE SAVEPOINT {savepoint name}`

You can also control the read/write systems of your transaction control system.

`SET TRANSACTION READ WRITE;`

`SET TRANSACTION READ ONLY;`

## Querying Data

There are four components to querying data, each with their own set of arguments that they accept: `SELECT`, `FROM`, `WHERE`, and `ORDER BY`.

`SELECT` accepts field names as its argument. Special parameters include `*` or `ALL` to include all columns, and `DISTINCT {Column name}` to not include duplicate records.

`FROM` accepts a database name as its argument. Remember if it is a database from another user you need to include the username as well (see the Database Objects section from earlier).

`WHERE` accepts condition/value pairs as its arguments where the condition is a field name. These can be combined using AND/OR expressions.

`ORDER BY` accepts a column name (or integer in its place) and order type as its arguments. Order types are `ASC` for ascending order, and `DESC` for descending order.

## Logical Operators in SQL

Logical operators are used in WHERE statements.

Equal `=`
Not Equal `!=`
Less Than `<`
Greater Than `>`
Less Than or Equal `<=`
Greater Than or Equal `>=`
`IS NULL`
`BETWEEN {x} AND {y}`
`IN ({x}, {y}, {z})`
`LIKE`
`EXISTS`
`UNIQUE`
`ALL`, `SOME`, and `ANY`
`AND`
`OR`
`NOT`

The `LIKE` operator uses two different kinds of wildcards. `_` is used for a single character, and `%` is used to represent one, several, or no characters.

Note that `NOT` can preface many logical operators, but for `IS NULL` you would use the more semantically palatable `IS NOT NULL`.

SQL also accepts basic arithmetic operators, `+`, `-`, `*`, and `/`.
