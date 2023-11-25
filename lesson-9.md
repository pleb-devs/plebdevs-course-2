## Lesson 9: Intro to SQL

###### What is SQL / Basic SQL commands / SQL Joins / SQL practice

### What is SQL?

**SQL** (Structured Query Language) is a language that allows us to "communicate" with databases. It breaks down as follows:

1. **Structured:** SQL has a defined format and syntax, which allows us to precisely describe our requirements from a database.
2. **Query:** A query is a request for data. We use queries to retrieve, insert, update, or delete data from a database.
3. **Language:** SQL is specifically designed to interact with relational databases.

Some commonly used relational database management systems (RDBMS) that use SQL include MySQL, Oracle, SQLite, PostgreSQL, and SQL Server. SQL enables tasks like retrieving, inserting, updating, and deleting data; creating and maintaining databases and tables; and managing access control.

### RDBMS

RDBMS (Relational Database Management System) organizes data across one or more tables, which can be related to each other. Examples include MySQL, Oracle Database, PostgreSQL, and SQLite.

### SQL & RDBMS

Key aspects of SQL and RDBMS:

1. **Relational Databases:** Data is stored in tables like spreadsheets, with rows representing records and columns representing fields. Tables link to others via common keys or attributes.
2. **Querying Data:** SQL allows for retrieving specific data based on certain criteria.
3. **Data Manipulation:** SQL can add (INSERT), change (UPDATE), or remove (DELETE) data.
4. **Database Management:** SQL includes commands for creating and modifying databases and tables (CREATE, ALTER, DROP).
5. **Access Control:** SQL manages who has access to data and their permissions (GRANT, REVOKE).
6. **Standardization:** SQL is standardized by ANSI and ISO, though many database systems add their proprietary extensions.
7. **Wide Adoption:** SQL is used across industries in various database systems.

### SQL Dialects

Each RDBMS uses its version of SQL, which may include unique functions, data types, operators, and optimizations for certain queries or operations.

# SQL Commands

“SQL: The language that is neither structured, nor strictly about queries, and certainly isn't limited to just language.” - A wise traveler

### SELECT Command

The SELECT command is used to select data from a database and store it in a result table (result-set). [Learn more](https://www.w3schools.com/sql/sql_select.asp)

### INSERT INTO Command

The INSERT INTO statement inserts new rows of data into a table. It can target specific columns or all columns. [Learn more](https://www.w3schools.com/sql/sql_insert.asp)

### UPDATE Command

The UPDATE statement modifies existing records in a table, often used with the WHERE clause to update specific rows. [Learn more](https://www.w3schools.com/sql/sql_update.asp)

### DELETE Command

The DELETE statement deletes existing records in a table, typically used with the WHERE clause to delete specific rows. [Learn more](https://www.w3schools.com/sql/sql_delete.asp)

### WHERE Clause

The WHERE clause filters records and is used with SELECT, UPDATE, and DELETE commands to operate on rows that meet a specified condition. [Learn more](https://www.w3schools.com/sql/sql_where.asp)

### CREATE Command

The CREATE command creates a Database or a Table within a database.

### ALTER Command

The ALTER command modifies the structure of an existing table in a database, such as adding, modifying, or deleting columns and constraints.

### DROP Command

The DROP command deletes an entire database or a table within a database. This action is irreversible.

### SQL Joins

SQL Joins combine records from two or more tables based on common values. Types of joins include INNER JOIN, LEFT (OUTER) JOIN, RIGHT (OUTER) JOIN, and FULL (OUTER) JOIN.

### Setting Up an Example SQL Database

For our “Users” and “Invoices” tables:

#### Users Table
```sql
CREATE TABLE Users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE,
    password TEXT,
    adminKey TEXT DEFAULT null
);
```

#### Invoices Table
```sql
CREATE TABLE Invoices (
    id INTEGER PRIMARY KEY,
    payment_request INTEGER UNIQUE,
    value INTEGER,
    memo TEXT,
    fees INTEGER,
    send BOOLEAN,
    settled BOOLEAN,
    settle_date DATETIME,
    created_at DATETIME DEFAULT (datetime('now')),
    userId INTEGER,
    FOREIGN KEY(userId) REFERENCES Users(id)
);
```
