---
title: "Using database tables in other environments"
teaching: 15
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

How and where the connection string information is provided depends on the ODBC driver and the application or program being used.

##  Connecting to Excel using ODBC

### Walkthrough example

1. Open an empty Excel workbook
2. From the Data ribbon select Get Data | From other Sources | From Microsoft Query
3. From the Choose Data Source window select `SQLite3 Datasource` and click **OK**.

  ![SQL_10_DataSource](../fig/SQL_10_DataSource.png)
  
  If this option does not appear in the list, then it probably means that the SQLite ODBC driver has not  been installed.
  
4. The 'Connect' window is where you specify the connection string information. You can see that there are many things that can be specified. But all we need to specify is the Database name. I.e the full path and name of the database file. There is a browse button you can use to navigate to the required file. When you have selected the database file click **OK**
  
5. At this point, you may get an a window pop up saying that there are no tables

![SQL_10_No_Tables](../fig/SQL_10_No_Tables.png)

Click **OK** . The Choose Columns Windows appears to support the 'no tables' assertion.

![SQL_10_Choose_Columns](../fig/SQL_10_Choose_Columns.png)

Click on the `options` button and then toggle the **system tables** option and click **OK**, then the tables will appear.


![SQL_10_Choose_Columns_2](../fig/SQL_10_Choose_Columns_2.png)

Select the SN7577 table and then click the '>' button to select all of the columns from the SN7577 table. This is the equivalent of the `Select *` SQL clause that we have used before. If you click the '+' button to the left of the table name, a full list of the column names is dsiplayed allowing you to select individual columns for inclusion. Click **OK**

6. 
## Connecting to Python or other programming environment using ODBC
