

## Setup

There is a seperate file for the setiup instructions for installing the various component used in this lesson. [setup](../setup.md)

* The DB Browser application
* SQLite Shell program
* SQLite ODBC connector

## The datasets used

The data from the SN7577 dataset is used. It has been placed into an SQLite database called SN7577.SQLite.
The data dictionary file for SN7577 is also referenced 'audit_of_political_engagement_11_ukda_data_dictionary.docx' a copy is also in the data folder and can be downloaded [here](../data/audit_of_political_engagement_11_ukda_data_dictionary.docx)
The [SN7577.tab](../data/SN7577.tab) and [SN7577_Text.csv](../data/SN7577_Text.csv) files are used when creating tables.
All of the files are in the data folder and can be downloaded using the links.

The SN7577.SQLite database file will need to be downloaded to the local machine before it can be opened by DB Browser.
It can be downloaded from [here](../data/SN7577.sqlite)



## The Lessons

If time is short, lessons 8 and 10 could be omitted as they don't intoduce any new SQL concepts.

[What is a Relational database](../_episodes/01-relational-database.md)

Explains terms like Relational database, Table, Datatype, Keys, Null

[Using DB Browser](../_episodes/02-db-browser.md)

This episode is a tour of the plugin and covers how to connect to an existing database, how to run a simple query and see the results.

[The Select statement](../_episodes/03-select.md)

SQL is explained and the different types of statements types included in SQL.
The basic components (select, from, where and sort) of the select statement are covered using the SN7577 table

[Missing data](../_episodes/04-missing-data.md)

In addition to NULL and how it is displayed in the plugin and relating it back to the actual dataset used to create the table,
There is a discussion of what else in the data could be a representation of missing data. e.g. -99 or -1.
Making use of the available data dictionary information.

[Creating new columns](../_episodes/05-creating-new-columns.md)

This episode cover the creation of new columns as part of a query. 
New columns are created from existing columns using builtin functions.
Changing datatypes of columns using `cast` is described along with how different datatypes appear in the plugin.
Both variants of the `case` statement are covered; to decide a value based on a decision and to allocate a value to a 'bin'.
The case statement is a s close as we get to more traditional programming, but it is a key part of SL available in almost all variants.
The use of Alias' are covered, in this case as a convenience rather than a necessity as they are in joins.


[Aggregations](../_episodes/06-aggregation.md)

The basic builtin aggregation functions and the `distinct` keyword are covered.
The use of the `group by` clause is explained as a way of providing more meaningful summaries across a table.
The `having` clause is introduced and compared to the `where` clause.

[Creating tables and views](../_episodes/07-creating-tables-views.md)

The various ways of creating tables are covered; From scratch, based on a query and by reading data from a file.
Adding data to table is demonstrated for the table created from scratch.
The table names may need to be changed to avoid existing names in the database. I would suggest prefixing with initiials.
The SQLite plugin is used to read the datasets which will need to have been copied to the local machine.
All of the options in the plugin wizard are explained.
Creating views, the close relationship to tables and guidance on naming views is covered.

[The SQLite command line](../_episodes/08_sqlite-command-lines.md)

Instructions on starting the command line shell are given. It is assumed that the setup instructions have already been completed.
If the sqlite3.exe has not been placed in the users path, then they eill either need to specify the full path to the executable or specify the full path to the database file when they try to connect. It is probably easier to ensure that the executable is in the path.
A comparison is made between the shell and the plugin GUI and reasons for wanting to use the shell are given.
Example of running queries are provided.
Use of 'dot' commands to change the output format are covered.
The use of 'dot' commands to save the output to a file is covered.
How to automate a script is covered.


[Joins](../_episodes/09-joins.md)

The need for table joins is discussed
The different types of joins is discussed and why you may need to do more than just inner joins to investigate your data.
There are examples of usingthe `join` and `on` SQL syntax. 
There is more discussion on Alias'.

[Using database tables in other environments](../_episodes/10-other-environments.md)

The episode requires that the ODBC driver has been installed.  It could be done as an Instructor demo only, but obviously that is not so much fun for the students.
The purpose and use of connection strings is covered 
There is a detailed demonstration of using ODBC from Excel to connect to the SN7577 database. It doesn't matter what table is used.
There are code only examples from R and Python. The aim is to emphasise the connection process and sending the query for execution rather than the code itself.
The code could be run in appropriate environments, but the database location would have to be changed.
