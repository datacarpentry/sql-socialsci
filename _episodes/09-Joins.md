---
title: "Joins"
teaching: 0
exercises: 0
questions:
- "What is meant by joining tables?"
- "Why would I want to join tables?"
- "What different types of joins are there?"
- "What are the consequences of using the wrong type of join?"

objectives:
- "Understand the structure of a joined table"

- "Familiarity with the different join types"

- "Use different join types in analysing your data"

- "Understand what other join types can tell you about your data"

keypoints:
- "First key point."
---

## About table joins

In any relational database system, the ability to join tables together is a key querying requirement.
Joins are used to combine the rows from two (or more) tables together to form a single table. A join between tables will only be possible if they have at least one column in common. The column doesn’t have to have the same name in each table, and quite often they won’t, but they do have to have a common usage.

For example the Q1 table has been created from the information about Q1 in the xxxx file. There are only two columns The value column indicates a respondents voting intentions and the key column has the value which is used to represent this intention in the SN7577 table.

You can see immediately from this that there is a connection or relationship between the the two tables. The key column in the Q1 table has been defined as a primary key. This guarantees that the key column has a unique set of values. Although not required, it is generally the case that the values in one of the columns that the tables have in common will have unique values in it. It is also not required but generally the case that the unique column will be a primary key in one of the tables.

In the SN7577 table, the values in the Q1 **column** are the key values in the Q1 table. You would not expect these to be unique, many respondents may have the same voting intentions.

If we wanted to find out the voting intentions of all of the respondents we could write a simple aggregation query like this;

~~~
SELECT Q1,
       count(*) as how_many
FROM SN7577
GROUP BY Q1
~~~~
{: .sql}

and this would give the results like this

| Q1 | how_many  |
|----|:----|
| 1	|179 |
| 2	|379|
| 3	|52|
| 4	|41|
| 5	|19|
| 6	|46|
| 7	|4|
| 8	|11|
| 9	|167|
| 10	|335|
| 11	|53|

What we would really like would be to have the **text** values of the voting intentions to make the results more readable.

We can achieve this by joining the Question1 table with the SN7577 table. 

Table names like column names can be given an alias. This is almost always done when joining tables. There are two reasons for doing this.

1. In the select clause you can make it clear from which table each of the selected columns comes from by prefixing the column name with the table alias followed by a period. You could use the full table name but by selecting short, single letter alias' you can save a lot of typing.
2. The column names that you wish to join on may have the same name and so you need some way of distinguishing between them.

Using simple alias' for the tables our join SQL looks like this;

~~~
SELECT q.value,
       count(*) as how_many
FROM SN7577 s
JOIN Question1  q
ON q.key = s.Q1
GROUP BY  s.Q1
~~~
{: .sql}

Here we have selected the 'value' column from the Question1 table as this has the text value that we want. The aggregation remains the same. The `JOIN` clause names the second table and the `ON` clause gives the criteria by which the tables are to be joined. The results obtained are given below.

|value | how_many |
|-----------------------------|:------|
|Conservative	| 179 |
|Labour	| 379 |
|Liberal Democrats (Lib Dem)	| 52 |
|Scottish/Welsh Nationalist	| 41 |
|Green Party	| 19 |
|UK Independence Party	| 46 |
|British National Party (BNP)	| 4 |
|Other	| 11 |
|Would not vote	| 167 |
|Undecided	| 335 |
|Refused	| 53 |

The results are the same as before except that the the text of the value column from the Questions1 table has replaced the numeric value from the Q1 column in the SN7577 table.

Notice that the the `GROUP BY` column does not appear in the `SELECT` clause.

> ## Exercise
>
> 1. Change the SQL above so that the group by clause is the value column from the Question1 table. Run the query. How are the results different?
> 2. Can you change the SQL so that table alias' are not used? Does this aid readability or not?
>
> > ## Solution
> > 
> > 1.  You only have to substitute **q.value** for **s.Q1** in the `GROUP BY` clause. Essentially the same results are returned but now they are in alphabetical order of the Quustion1 value column text.
> > 
> > 2. Because there are no conflicts in the column names between the two tables you could write the query without using alias' for the tables. Including the alias' adds clarity and readability to the SQL, especially if you are selecting several columns from each of the tables.
> > 
> {: .solution}
{: .challenge}



## Different join types

The example of a join given above is called an **INNER ** join, we could have written **INNER JOIN ** rathere than simply **JOIN**. This is almost never done in practice as the inner join is by far the most common join type used.

Other Join types are available...

Before we look at the other join types we need to explain how the **Inner join** works and why it is so commonly used. 

As for all joins we are defining a relationship between two tables based on the data values in two columns, one from each table. What that relationship is, is  given by the criteria in the `ON` clause. The value of the column in one table must be same as that in the other table.



There are severeal different join types possible

Using different join types in analysing your data

