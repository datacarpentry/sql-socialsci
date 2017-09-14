---
title: "Using database tables in other environments"
teaching: 0
exercises: 0
questions:
- "How do I save my query results for use by other programs or applications?"
- "What are and how do I use ODBC applications?"
- "How can I access an SQLite database table from other programming environments?"

objectives:
- "Understand what ODBC is and when it can be used"
- "Construct appropriate connection strings"
- "Appreciate the advantages of using ODBC to access a remote database"
- "Use an ODBC connection from Excel to retrieve data from a database"
- "Use ODBC from Python or other programming environment to retrieve data from a database"

keypoints:
- "ODBC - Open DataBase Connector allows a database to be connected to a program or application"
- "Each database system has its own ODBC connectors"
- "Programs such as Excel allow you to use ODBC to get data from databases"
- "Programming languages such as Python and R provide libraries which facilitate ODBC connections"
---

## ODBC and advantages of using it

ODBC  - Open Database Conectivity (or Connector) is a piece of software, often referred to as a *driver*, which allows a database to be connected to an application or program. ODBC drivers are specific to a given database system. As we are using an `SQLite` database we need an SQLite specific ODBC driver to connect to it.

The installation of the SQLite ODBC driver for a Windows machine is explained in the [SQL setup document](/xxxx) . 

So far in this lesson we have accessed our SQLite database either through the Firefox SQLite plugin or directly using the commandline shell. Each of these methods have their own advanatages. The plugin provides a simple GUI (Graphical User Interface), for development and testing new queries. The shell aids automation of tasks such as adding rows to a table or allowing whole scripts of SQL commands to be run consequetively without user intervention. In both of these methods, we have seen that the 'outputs' can be saved to `csv` files from where they can be read into other applications or programs for futher processing. Using ODBC missies out the middle man. The application of program connects directly to the SQLite database, send it an SQL query, recieves the output from that query and processes the results in an appropriate fashion.

In the case of Excel the tabular results of the query are displayed in a worksheet. For R and Python the results are assigned to a suitable variable from where they can be examined or further processed.

In the following sections we will give examples of using ODBC to connect to Excel and also R and Python.




## Connection strings

A connection string is really just a list of parameters and their values which explain to the ODBC driver what database you wish to connect to how you wish to use it. For some database systems this might involve providing user credentials as well as specifying which particular database you want to access. For our use of SQLite, the connection string is essentially the full pathname and filename of the SQLite database file.


##  Connecting to Excel using ODBC

## Connecting to Python or other programming environment using ODBC
