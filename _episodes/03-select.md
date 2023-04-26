---
title: "The Select Statement"
teaching: 15
exercises: 10
questions:
- "What is SQL?"
- "How can I return specific columns from a table?"
- "How can I return specific rows from a table?"

objectives:
- "Define  SQL" 
- "Explain how SQL is used to access relational database tables"
- "Understand the difference  between DDL and DML"
- "Create simple SQL queries to return rows and columns from existing tables"
- "Construct more complex logical expressions for use in WHERE clauses"
- "Return sorted results from a query"

keypoints:
- "Strictly speaking SQL is  a standard, not a particular implementation"
- "SQL implementation are sufficiently close that you only have to learn SQL once"
- "The DDL constructs are used to create tables and other database objects"
- "The DML constructs, typically the SELECT statement is used to retrieve data from one or more tables"
- "The SELECT statement allows you to 'slice' and 'dice' the columns and rows of the dataset so that the query only returns the data of interest"
---

## Definition of SQL 
SQL or Structured Query Language is an international standard for manipulating data in a relational database.
Each Relational Database system like Oracle, MySQL or SQLite implements its own variation of the standard.

Fortunately for the types of commands and queries that we will want to write, all of the implementations are much in agreement.
The SELECT queries we will be writing to access data in our SQLite database will execute un-altered in many of the other environments.

Essentially you only have to learn SQL once.

## SQL and Relational database tables

The strength of SQL is that a single SQL statement or query can request data be returned from one or many of the tables in the database. 
You can essentially define the relationships between tables on-the-fly as part of your query statement. Relationships between tables are often included as
part of the overall database design. In our situation we may be getting an assortment of tables from different sources so being able to imply the relationship as part of 
the query has definite advantages.

## DDL and DML

DDL stands for Data Definition Language. It is the set of SQL commands used to create alter of delete database objects such as tables.

DML stands for Data Manipulation Language. For our purposes this is the SELECT command which is used to extract data items from one or more of the database tables.

## Simple SQL queries using the Select statement

For the rest of this episode we will be looking at the SELECT statement.

To follow along, you should open the DB Browser application and connect to the SQL_SAFI database.

In SQL, querying data is performed by a SELECT statement. A select statement has 6 key components;

~~~ 
SELECT colnames
FROM tablename
WHERE conditions
GROUP BY colnames
HAVING conditions
ORDER BY colnames
~~~ 
{: .sql}

In practice very few queries will have all of these clauses in them simplifying many queries. On the other hand, 
conditions in the WHERE clause can be arbitrarily complex and if you need to JOIN two or more tables together then more clauses (JOIN and ON) are needed.

All of the clause names above have been written in uppercase for clarity. SQL is not case sensitive. Neither do you need to write each clause on a new line, but it is often clearer to do so for all but the simplest of queries.

In this episode we will start with the very simple and work our way up to the more complex.

The simplest query is effectively one which returns the contents of the whole table

~~~ 
SELECT *
FROM Farms;
~~~ 
{: .sql}

It is better practice and generally more efficient to explicitly list the column names that you want returned.

~~~ 
SELECT Country, A06_province, A07_district, A08_ward, A09_village
FROM Farms;
~~~ 
{: .sql}

The '*' character acts as a wildcard meaning all of the columns but you cannot use it as a general wildcard.
So for example, the following is not valid.
~~~ 
SELECT A*
FROM Farms;
~~~ 
{: .sql}

If you run it you will get an error.
When an error does occur you will see an error message displayed in the bottom pane. 

In addition to limiting the columns returned by a query, you can also limit the rows returned. 
The simplest case is to say how many rows are wanted using the `LIMIT` clause.
In the example below only the first ten rows of the result of the query will be returned. 
This is useful if you just want to get a feel for what the data looks like. 

~~~ 
SELECT *
FROM Farms
LIMIT 10;
~~~ 
{: .sql}

> ## Exercise
>
> Write a query which returns the first 5 rows from the Farms table with only the columns Id, and B16 to B20.
> 
> > ## Solution
> >  ~~~
> > SELECT  Id
> >       , B16_years_liv
> >     , B17_parents_liv
> >     , B18_sp_parents_liv
> >     , B19_grand_liv
> >     , B20_sp_grand_liv
> > FROM Farms
> > LIMIT 5;
> > ~~~
> > {: .sql}
> > Because the query uses several columns (with longish names), for readability they have been set out on separate lines. SQL takes 
> > of white space to you are free to arrange the text of the query as you like.  
> {: .solution}
{: .challenge}

## The `WHERE` clause

Usually you will want to restrict the rows returned based on some criteria. i.e. certain values or ranges within one or more columns. 

In this example we are only interested in rows where the value in the B16_years_liv column is greater than 25

~~~
SELECT  Id, B16_years_liv
FROM Farms
WHERE B16_years_liv > 25
;
~~~ 
{: .sql}

In addition to using the '>' we can use many other operators such as <, <=, =, >=, <> 
~~~
SELECT  Id, B17_parents_liv
FROM Farms
WHERE B17_parents_liv = 'yes'
;
~~~ 
{: .sql}

## Using more complex logical expressions in the `WHERE` clause

We can also use the AND and OR keywords to build more complex selection criteria. 

~~~
SELECT  Id
FROM Farms
WHERE    B17_parents_liv = 'yes' 
     AND B18_sp_parents_liv = 'yes' 
     AND B19_grand_liv = 'yes' 
     AND B20_sp_grand_liv = 'yes' 
;
~~~ 
{: .sql}

Notice that the columns being used in the `WHERE` clause do not need to returned as part of the `SELECT` clause. 

You can ensure the precedence of the operators by using brackets. Judicious use of brackets can also aid readability

~~~
SELECT  Id
FROM Farms
WHERE (B17_parents_liv = 'yes' OR B18_sp_parents_liv = 'yes') AND B16_years_liv > 60
;
~~~ 
{: .sql}

> ## Exercise
>
> From the above query, breakdown the `WHERE` clause so that each component can be tested individually.
> Make a note of how many rows are returned in each case.
> 
> > ## Solution
> > 
> > To test each of the `OR` clauses
> > 
> > ~~~
> > SELECT  Id
> > FROM Farms
> > WHERE B17_parents_liv = 'yes'
> > ;
> > SELECT  Id
> > FROM Farms
> > WHERE B18_sp_parents_liv = 'yes'
> > ;
> > SELECT  Id
> > FROM Farms
> > WHERE (B17_parents_liv = 'yes' OR B18_sp_parents_liv = 'yes') 
> > ;
> > SELECT  Id
> > FROM Farms
> > WHERE B16_years_liv > 60
> > ;
> > ~~~
> > {: .sql}
> > 
> > `OR` generally creates a less restrictive condition and `AND` makes a more restrictive condition.
> > 
> {: .solution}
{: .challenge}

The following query returns the rows where the value of B16_years_liv is in the range 51 to 59 inclusive.

~~~
SELECT Id, B16_years_liv
FROM Farms
WHERE B16_years_liv > 50 AND B16_years_liv < 60
;
~~~ 
{: .sql}


The same results could be obtained by using the BETWEEN or IN operators

~~~
SELECT Id, B16_years_liv
FROM Farms
WHERE B16_years_liv BETWEEN 51 AND 59
;
~~~ 
{: .sql}


~~~
SELECT Id, B16_years_liv
FROM Farms
WHERE B16_years_liv IN (51, 52, 53, 54, 55, 56, 57, 58, 59)
;
~~~ 
{: .sql}

The list of values in brackets do not have to be contiguous or even in order.

> ## Exercise
>
> Write a query using the Farms table which returns the columns Id, A09_village, A11_years_farm, B16_years_liv. We are only interested in rows where the A09_village value is either 'God' or 'Ruaca'. Additionally
> we only want A11_years_farm values in the range 20 to 30 exclusive and B16_years_liv values strictly greater than 40.
> There are many ways of doing this, but try to use an inequality, an `IN` clause and a `BETWEEN` clause.
> 
> > ## Solution
> > 
> > ~~~
> > SELECT Id, A09_village, A11_years_farm, B16_years_liv
> > FROM Farms
> > WHERE     A09_village IN ('God', 'Ruaca') 
> >       AND A11_years_farm BETWEEN 21 AND 29 
> >       AND B16_years_liv > 40
> > ;
> > ~~~
> > 
> {: .solution}
{: .challenge}



## Sorting results 

If you want the results of your query to appear in a specific order, you can use the ORDER BY clause

~~~
SELECT Id, A09_village, A11_years_farm, B16_years_liv
FROM Farms
WHERE A09_village = 'God'
ORDER BY A11_years_farm
;
~~~ 
{: .sql}

By default the SQL assumes Ascending order. You can make this more explicit by using the `ASC` or `DESC` keywords.

~~~
SELECT Id, A09_village, A11_years_farm, B16_years_liv
FROM Farms
WHERE A09_village = 'God'
ORDER BY A11_years_farm DESC
;
~~~ 
{: .sql}

You can also order by multiple columns

~~~
SELECT Id, A09_village, A11_years_farm, B16_years_liv
FROM Farms
WHERE A09_village = 'God'
ORDER BY A11_years_farm DESC , B16_years_liv ASC
;
~~~ 
{: .sql}
 
