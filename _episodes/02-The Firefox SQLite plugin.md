---
title: "The Firefox SQLite plugin"
teaching: 0
exercises: 0
questions:
- "What does the Firefox SQLite plugin allow me to do?"
objectives:
- "Understand the layout of the Firefox SQLite plugin and the key facilities that it provides"

- "Connect to databases"

- "Create new databases and tables"

- "Run SQL queries"

- "Export the results of queries"

keypoints:
- "First key point."
---

## The layout of the Firefox SQLite plugin 

Assuming you have followed the instructions in the setup document correctly, when you select the SQLite Manager option from the tools menu in the Firefox browser, the SQLite manager will open in a new window, just like a normal Windows application.

We will look at the toolbar items and menu options as we need them.


In the left hand pane there will be a short message saying "> Database not Selected". This pane will be used to display information about your database and the tables within it once we have connected to it.

The righthand pane is tabbed with the "Execute SQL" tab selected by default this is where we will type in our SQL queries and see the results of running the query. But we cannot do this until we first connect to a database.


## Connecting to databases
When you click on the Database menu item amongst the options available will be to create a new database or to connect to an existing database.

When you take the connect to an existing database option you will be presented with a standard Windows file open dialog, allowing you to navigate to your database file and select it. Once you have connected to the database will be populated with various pieces of information about the database. Mainly what it contains. 

We will mainly be interested in the Tables item, which like all of the others is a tree like structure which initially opens to show all of the tables in your base. In our example, there is only one; SN7577. 

If you double click on the table name, the tree expands to show you all of the column names in the table. 

The Execute SQL tab in the right hand pane also changes once you connect to a database. It now has a section at the top for you to write an SQL Query and a section at the bottom where the results of you query will appear. In between the two there are two buttons; Run SQL - to run your query and Actions which relates to the output from the query which we will look at later. 

Should there be a problem with your query when you try to run it, the resulting error message is displayed in the Last Error: text box.

Aside - The code snippet tabs

## Running SQL queries


~~~ 
Select *
From tablename
~~~ 
{: .sql}

~~~ 
Select *
From SN7577
~~~ 
{: .sql}

~~~ 
Select web1, web2, web3, web4
From SN7577
~~~ 
{: .sql}

The '*' character acts as a wildcard meaning all of the columns but you cannot use it as a general wildcard.
So for example, the following is not valid.
~~~ 
Select w*
From SN7577
~~~ 
{: .sql}

If you run it you will get an error.
When an error does occur you will see an alert box pop-up with a rather technical an unreadable error message in it. How ever when you close it, you will see a far simpler interpretation of the error in the Last Error text box. 




## Exporting the results of queries

## Creating new databases and tables
