+++
title = "MS SQL"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

SQL Server : Relational Database Management System

RDMS

- Data SEcurity
- Data Integrity
- Referential Integrity

ERD (An Entity Relationship Diagram)

- Entity : Tables
- Entity Properties : Columns

Each table has a primary key

- Primary key : Unique value on every table
- Foreign Key : Defines a relationship to another table

## Conventions

- Table Name : PascalCase
- Table Property (Column), Attribute : camelCase
- Row : Tuple
- Relation/Association
- Schema

## SQL

SQL is Structured Query Language, is used to interact with the Database Engine.

**Create a Database**

```sql
CREATE DATABASE Name;
```

**Delete a Database**

```sql
DROP DATABASE Name;
```

#### Data Types

| Type          | Description                                           |
| ------------- | ----------------------------------------------------- |
| INT           | -2,147,483,648 to 2,147,483,647                       |
| DECIMAL(p,s)  | p defaults to 18, s defaults to 0                     |
| NUMERIC(p,s)  | same as decimal                                       |
| BIT           | Integer 0, 1, or null                                 |
| MONEY         | -922,337,203,685,477.5808 to 922,337,203,685,477.5807 |
| DATE          | 0001-01-01 ~ 9999-12-31                               |
| TIME          | hours minuites and secounds on a 24 hour clock        |
| DATETIME      | date + time                                           |
| CHAR(size)    | fixed length with a max of 8,000 characters           |
| VARCHAR(size) | varaible length with a max size of 8,000              |

- p (precision) : The maximum total number of decimal digits to be stored.
- s (scale) : The number of decimal digits that are stored to the right of the decimal point.

SELECT - extracts data from a database
UPDATE - updates data in a database
DELETE - deletes data from a database
INSERT INTO - inserts new data into a database
CREATE DATABASE - creates a new database
ALTER DATABASE - modifies a database
CREATE TABLE - creates a new table
ALTER TABLE - modifies a table
DROP TABLE - deletes a table
CREATE INDEX - creates an index (search key)
DROP INDEX - deletes an index

#### Convention

- Database name : PascalCase
- Table name : PascalCase, singular (?)
- Column name : camelCase, singular

## Create Table
