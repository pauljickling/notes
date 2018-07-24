# SQL

SQL stands for *Structured Query Language*, and it is the standard language used to communicate with relational databases. There are many flavors of SQL available (MySQL, PostgresSQL, SQLite, etc.) These notes will be referencing PostGresSQL.

## Relational Databases

A relational database is a database that contains tables, each table contains fields (essentially the model schemas), and rows of data (the object properties). Every table will also have a row that contains a primary identification key, and different tables can form relationships by sharing these keys.

## Basics

Starting up SQL:

`sudo su -postgres`

Creating a database:

`createdb {db name}`

Entering SQL shell:

`psql`

Exiting shell:

`\q`

Exiting SQL:

`exit`
