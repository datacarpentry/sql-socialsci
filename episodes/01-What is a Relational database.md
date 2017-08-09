---
title: "What is a relational database?"
teaching: 0
exercises: 0
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
- "First key point."
---

## What is a relational database?

A relational database is a collection of data items organised as a set of tables. Relationships can be defined between the data in one table and the data in another or many other tables. The relational database system will provide mechanisms by which you can query the data in the tables, re-assemble the data in various ways without altering the data in the actual tables. 
This querying is usually done using SQL (Structured Query Language). This is a relatively straight forward language to learn, certainly for simple queries.
You could have a relational database with only one table, but then you wouldn’t have any relationships and it would be more like a spreadsheet. 
Databases are designed to allow efficient querying against very large tables, more than the 1M rows allowed in an Excel spreadsheet.

## What is a table?

As were have noted above , a single table is very much like a spreadsheet. It has rows and it has columns. A row represents a single observation and the columns represents the various variables contained within that observation. 
The rows may or may not have an index or key column as a means of uniquely identifying a row.
The columns typically have a name associated with them indicating the variable name, or in one of the examples we are going to use a question number from a survey. A column always represents the same variable for each row contained in the table. Because of this the data in each column will always be of the same *type*, such as an Integer, or Text value for all rows in the table. Datatypes are discussed in the next section.



## What is a data type?

A data type is a description of the kind of data in a table column. Typical examples will be Integer or Text. In SQLite there is quite a number of possible data type.

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

Most of the others are used to allow SQLite to more precisely set aside storage space for the data. 

## Why do tables have key columns?

Whenever you create a table, you will have the option of designating one of the columns as the primary key column. The main property of the primary key column is that the values contained in it must uniquely identify that particular row. That is you cannot have duplicate primary keys. This can be an advantage which adding rows to the table as you will not be allowed to add the same row (or  a row with the same primary key) twice.

The primary key column for a table is usually of type Integer although you could have Text. For example if you had a table of car information, then the "Reg_No" column could be made the primary key as it can be used to uniquely identify a particular row in the table.

A table doesn't have to have a primary key although they are recommended for larger tables. 


## What different types of keys are there?



## How does the database represent missing data?
