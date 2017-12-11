---
title: "The Select Statement"
teaching: 15
exercises: 5
questions:
- "What is SQL?"
- "How can I return specific columns from a table?"
- "How can I return specific rows from a table?"

objectives:
- "Define  SQL" 

- "Explain how SQL is used to access relational database tables"
- "Understand the difference  between DDL and DML"
- "Create simple SQL queries to return rows and columns from existing tables"
- "Construct more complex logical expressions for use in where clauses"
- "Return sorted results from a query"

keypoints:
- "Strictly speaking SQL is  a standard, not a particular implementation"
- "SQL implementation are sufficiently close that you only have to learn SQL once"
- "The DDL constructs are used to create tables and other database objects"
- "The DML constructs, typically the SELECT statement is used to retrieve data from one or more tables"
- "The SELECT statement allows you to 'slice' and 'dice' the columns and rows of the dataset so that the query only returns the data of interest"
---

## Definition of SQL 
SQL or Structured Query Language is an internatinal standard for manipulating data in a relational database.
Each Relational Database system like Oracle, MySQL or SQLite implements its own variation of the standard.

Fortunateley for the types of commands and queries that we will want to write, all of the implementations are much in agreement.
The relatively straightforward Select queries we will be writing to access data in our SQLite database will execute un-altered in many of the other environments.

Essentially you only have to learn SQL once.

## SQL and Relational database tables

The strength of SQL is that a single SQL statement or query can request data be returned from one or many of the tables in the database. You can essentially define the relationships between tables on-the-fly as part of your query statement.

## DDL and DML

DDL stands for Data Definition Language. It is the set of SQL comands used to create alter of delete database objects such as tables.

DML stands for Data Manipulation Language. Principally this is the SELECT command which is used to extract data items from one or more of the database tables.

## Simple SQL queries useing the Select statement

For the rest of this episode we will be looking at the SELECT statement.

To follow along, you should open the DB Browser application and connect to the SN7577 database.

In SQL, querying data is performed by a SELECT statement. A select statement has 6 key components;

~~~ 
SELECT colnames
FROM tablename
GROUP BY colnames
WHERE conditions
HAVING conditions
ORDER BY colnames
~~~ 
{: .sql}

In practice very few queries will have all of these clauses in them simplifying many queries. On the other hand, conditions in the WHERE clause can be very complex and if you need to JOIN two or more tables together then more clauses (JOIN and ON) are needed.

All of the clause names above have been written in uppercase for clarity. SQL is not case sensitive. Neither do you need to write each clause on a new line, but it is often clearer to do so for all but the simplest of queries.

In this episode we will start with the very simple and work our way up to the more complex.

The simplest query is effectively one which returns the contents of the whole table

~~~ 
Select *
From SN7577;
~~~ 
{: .sql}

It is better practice and generally more efficient to explicitly list the column names that you want returned.

~~~ 
Select web1, web2, web3, web4
From SN7577;
~~~ 
{: .sql}

The '*' character acts as a wildcard meaning all of the columns but you cannot use it as a general wildcard.
So for example, the following is not valid.
~~~ 
Select w*
From SN7577;
~~~ 
{: .sql}

If you run it you will get an error.
When an error does occur you will see an error message displayed in the bottom pane. 

In addition to limiting the columns returned by a query, you can also limit the rows returned. The simplest case is to say how many rows are wanted using the `Limit` clause. In the example below only the first ten rows of the result of the query will be returned. This is useful if you just want to get a feel for what the data looks like. 

~~~ 
Select *
From SN7577
Limit 10;
~~~ 
{: .sql}

> ## Exercise
>
> Write a query which returns the first 5 rows from the SN7577 table with only the columns Q1,Q2,Q3,Q4 and numage.
> 
> > ## Solution
> >  ~~~
> >  Select Q1, Q2, Q3, Q4, numage
> >  From SN7577
> >  Limit 5;
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}

## The `Where` clause

Usually you will want to restrict the rows returned based on some criteria. i.e. certain values or ranges within one or more columns. 

In this example we are only interested in rows where the value in the Q1 column is 2

~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 = 2;
~~~ 
{: .sql}

In addition to using the '=' we can use many other operators such as <, <=, >, >=, <> 
~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 > 5;
~~~ 
{: .sql}

## Using more complex logical expressions in the `Where` clause

We can also use the AND and OR keywords to build more complex selection criteria. 

~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 > 5 and Q3 <> 2;
~~~ 
{: .sql}

You can ensure the precedence of the operators by using brackets. Judicious use of brackets can also aid readability

~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 = 5 or (Q3 <> 2 and Q4 > 8);
~~~ 
{: .sql}

The following query returns the rows where the value of Q1 either 5,6,7 or 8

~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 > 5 and Q1 < 8;
~~~ 
{: .sql}


The same results could be obtained by using the BETWEEN or IN operators

~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 Between 5 and 8;
~~~ 
{: .sql}


~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 In (5, 6, 7, 8);
~~~ 
{: .sql}

The list of values in brackets do not have to be contiguous or even in order.

> ## Exercise
>
> In the SN7577 table the values of the web1, web2, web3, web4 columns are all -1, 0 or 1. The numage columns can be any value between 0 and 95.
> Write a query which returns the web1, web2, web3, web4 and numage columns from the SN7577 table. The web1, web2, web3, web4 values should all be either 0 or 1 and the numage values should be between 30 and 40 inclusive.
> There are many ways of doing this, but try to use two different inequalities, an `IN` clause and a `BETWEEN` clause.
> 
> > ## Solution
> > 
> > ~~~
> > select web1, web2, web3 ,  numage
> > from sn7577
> > where web1 >= 0 and web2 in (0, 1) and web3 <> -1 and numage between 30 and 40;
> > ~~~
> > 
> {: .solution}
{: .challenge}



## Sorting results 

If you want the results of your query to appear in a specific order, you can use the ORDER BY clause

~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 In (5, 6, 7, 8)
Order By Q1;
~~~ 
{: .sql}

By default the SQL assumes Ascending order. You can make this more explicit by using the ASC or  DESC keywords.

~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 In (5, 6, 7, 8)
Order By Q1 Desc;
~~~ 
{: .sql}

You can also order by multiple columns

~~~
Select  Q1,  Q3, Q4
From SN7577
Where Q1 In (5, 6, 7, 8)
Order By Q1 Desc, Q3 Asc;
~~~ 
{: .sql}
