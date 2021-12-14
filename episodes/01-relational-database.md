---
title: "What is a relational database?"
teaching: 10
exercises: 5
questions:
- "What is a relational database?"
- "What is a table?"
- "What is a data type?"
- "Why do tables have key columns?"
- "What different types of keys are there?"
- "How does the database represent missing data?"

objectives:
- "Define a relational database"
- "Compare with other types of databases"
- "Understand the  structure of a table"
- "List the SQLite datatypes"
- "Explain the purpose of a Schema"
- "Explain Key fields"
- "Understand the use of NULL"

keypoints:
- "A relational database is data organised as a collection of related tables"
- "SQL (Structured Query Language) is used to extract data from the tables. Either a single table or data spread across two or more related tables."
- "A schema, which describes the data in a table, has to be created before data can be added"
- "The schema can be used to provide some data validation on input"
---

## What is a relational database?

A relational database is a collection of data items organised as a set of tables. Relationships can be defined between the data in one table and the data in another or many other tables. The relational database system will provide mechanisms by which you can query the data in the tables, re-assemble the data in various ways without altering the data in the actual tables. 
This querying is usually done using SQL (Structured Query Language). SQL allows a great many queries to be constructed from the use of only a few keywords.
You could have a relational database with only one table, but then you would’t have any relationships and it would be more like a spreadsheet. 
Databases are designed to allow efficient querying against very large tables, more than the 1M rows allowed in an Excel spreadsheet.

## What is a table?

As were have noted above, a single table is very much like a spreadsheet. It has rows and it has columns. A row represents a single observation and the columns represents the various variables contained within that observation. 
Often one or more columns in a row will be designated as a 'primary key' This column or combination of columns can be used to uniquely identify a specific row in the table. 
The columns typically have a name associated with them indicating the variable name. A column always represents the same variable for each row contained in the table. Because of this the data in each column will always be of the same *type*, such as an Integer or Text, of values for all of the rows in the table. Datatypes are discussed in the next section.



## What is a data type?

A data type is a description of the kind of data in a table column. Each database system recognises its own set of datatypes, although some are common to many.
Typical examples will be Integer or Text. 

The table below gives some examples.

| Data type                          | Description                                                                                              |
|------------------------------------|:----------------------------------------------------------------------------------------------------|
| CHARACTER(n)                       | Character string. Fixed-length n                                                                         |
| Text                               | Character string. Variable length                                                                        |
| VARCHAR(n) or CHARACTER VARYING(n) | Character string. Variable length. Maximum length n                                                      |
| BINARY(n)                          | Binary string. Fixed-length n                                                                            |
| BOOLEAN                            | Stores TRUE or FALSE values                                                                              |
| VARBINARY(n) or BINARY VARYING(n)  | Binary string. Variable length. Maximum length n                                                         |
| INTEGER(p)                         | Integer numerical (no decimal).                                                                          |
| SMALLINT                           | Integer numerical (no decimal).                                                                          |
| INTEGER                            | Integer numerical (no decimal).                                                                          |
| BIGINT                             | Integer numerical (no decimal).                                                                          |
| DECIMAL(p,s)                       | Exact numerical, precision p, scale s.                                                                   |
| NUMERIC(p,s)                       | Exact numerical, precision p, scale s. (Same as DECIMAL)                                                 |
| FLOAT(p)                           | Approximate numerical, mantissa precision p. A floating number in base 10 exponential notation.          |
| REAL                               | Approximate numerical                                                                                    |
| FLOAT                              | Approximate numerical                                                                                    |
| DOUBLE PRECISION                   | Approximate numerical                                                                                    |
| DATE                               | Stores year, month, and day values                                                                       |
| TIME                               | Stores hour, minute, and second values                                                                   |
| TIMESTAMP                          | Stores year, month, day, hour, minute, and second values                                                 |
| INTERVAL                           | Composed of a number of integer fields, representing a period of time, depending on the type of interval |
| ARRAY                              | A set-length and ordered collection of elements                                                          |
| MULTISET                           | A variable-length and unordered collection of elements                                                   |
| XML                                | Stores XML data       

But in practice you can usually restrict your usage to a few 

| Data type                          | Description                                                   |
|------------------------------------|:--------------------------------------------------------------|
| BOOLEAN                            | Stores TRUE or FALSE values                                   |
| INTEGER                            | Integer numerical (no decimal).                               |
| FLOAT                              | Approximate numerical                                         |
| DATE                               | Stores year, month, and day values                            |
| TIME                               | Stores hour, minute, and second values                        |
| TIMESTAMP                          | Stores year, month, day, hour, minute, and second values      |                                                                     

In SQLite there is only a small number.

| Data type                          | Description                                                   |
|------------------------------------|:--------------------------------------------------------------|
| NULL                               | The value is a NULL value                                     |
| INTEGER                            | The value is a signed integer, stored in 1, 2, 3, 4, 6,       |
|                                    | or 8 bytes depending on the magnitude of the value            |
| REAL                               | The value is a floating point value, stored in 8-bytes        |
| TEXT                               | The value is a text string                                    |
| BLOB                               | The data is stored exactly as it was input, Used for binary   |
|                                    | data such as images.                                          |

We won't be using any BLOB data and it is debatable whether or not NULL should be considered a type at all.

There are some common datatypes which are missing from the SQLite list.

BOOL or BOOLEAN : This type typicaly accepts values of 'True' and 'False' In SQLite we would use the Integer type and assign vlaues of 1 to represent 'True' and 
0 to represent 'False'.

DATE, DATETIME, TIMESTAMP : SQLite does not have a datatype for storing dates and/or times. You can use TEXT, REAL, or INTEGER values
for these and use the built-in Date And Time Functions to manipulate them. We will look at manipulating dates in Lesson 5.



## Why do tables have primary key columns?

Whenever you create a table, you will have the option of designating one of the columns as the primary key column. The main property of the primary key column is that the values contained in it must uniquely identify that particular row. That is you cannot have duplicate primary keys. This can be an advantage which adding rows to the table as you will not be allowed to add the same row (or  a row with the same primary key) twice.

The primary key column for a table is usually of type Integer although you could have Text. For example if you had a table of car information, then the "Reg_No" column could be made the primary key as it can be used to uniquely identify a particular row in the table.

A table doesn't have to have a primary key although they are recommended for larger tables. A primary key can also be made up of more than one column, although this is less usual.


## What different types of keys are there?

In addition to the primary key, a table may have one or more _Foreign keys_. A foreign key does not have to be unique or identified as a foreign key when the table is created. A foreign key in one table will relate to the primary key in another table. This allows a relationship to be created between the two tables. If a table needs to be related to several other tables, then there will be a foreign key  (column) for each of those tables.

## How does the database represent missing data?

All relational database systems have the concept of a NULL value. NULL can be thought of as being of all data types or of no data type at all. It represents something which is simply _not known_.

When you create a database table, for each column you are allowed to indicate whether or not it can contain the NULL value. Like primary keys, this can be used as a form of data validation.

In many real life situations you will have to accept that the data isn't perfect and will have to allow for NULL or missing values in your table.

In DB Browser we can indicate how we want NULL values to be displayed. We will use a RED background to the cell to make it stand out. In SQL queries you can specifically test for NULL values.

We will look at missing data in more detail in a later episode.

