---
title: "Joins"
teaching: 20
exercises: 10
questions:
- "What is meant by joining tables?"
- "Why would I want to join tables?"
- "What different types of joins are there?"
- "How do Joins help you discover missing data or gaps in the data"

objectives:
- "Understand the structure of a joined table"

- "Familiarity with the different join types"

- "Use different join types in analysing your data"

- "Understand what other join types can tell you about your data"

keypoints:
- "Joins are used to combine data from two or more tables."
- "Tables to be joined must have a column in each which represent the same thing"
- "There are several different types of joins"
- "The Inner join is the most commonly use"
- "You may have to use the other join types to discover missing data or gaps in your data"
---

## About table joins

In any relational database system, the ability to join tables together is a key querying requirement.
Joins are used to combine the columns from two (or more) tables together to form a single table. 
A join between tables will only be possible if they have at least one column in common. 
The column doesn’t have to have the same name in each table, and quite often they won’t, but they do have to have a common usage.


In the SAFI database we have three tables. Farms, Plots and Crops. Each farm has a number of plots (or fields) and each plot can be used to grow differnt crops.
A question you might ask is: Which Farms with more than 12 people in the hosehold grow Maize? No single table has the answer to this question.

We can write queries to answer each part seperately




~~~
-- how many crops of Maize?
select * 
from Crops
where D_curr_crop = 'maize'
;
~~~~
{: .sql}


![Maize](../fig/SQL_09_maize.png)

and

~~~
-- Which farms have more than 12 in the Houshold
select Id, B_no_membrs 
from Farms
where B_no_membrs > 12
;
~~~~
{: .sql}

![Maize](../fig/SQL_09_maize.png)

In order to answer the question we need information from both tables at the time, i.e. from a single query.
Notice that in the tables returned by both of the above queries we have the `Id` column.
This column represents the Household or Farm in both of the tables. Because of this we can use this `Id` column from both tables to `join`
the tables together.  

Providing we are confident that both of the columns represent the household (or farm) it doesn't matter whether or not they have the same name.

We write a `join` query like this:

~~~
select a.Id, a.B_no_membrs,
       b.Id, b.D_curr_crop
from Farms as a
join Crops as b
on a.Id = b.Id and a.B_no_membrs > 12 and b.D_curr_crop = 'maize'
;
~~~
{: .sql}

There are several things to notice about this query:

1. We have used alias' for the the table names in the same way as we used with columns in a previous lesson. In this case though, it is not to povide
more meaningful names, in fact alias' for tables are often single letters to save key strokes.
2. We use the table alias as a prefix, plus a '.' when we refer to a column name from the table. You don't have to do this, but it generally adds clarity to the query.
3. You will need to use an alias when you need to refer to a column with the same name in both tables. In our case we need to compare the `Id` column in both tables.
4. In the select clause, we list all of the columns, from both table that we want in the output. We use tha alias' for clarity. If the column name is not ambiguous, i.e it only occurs in one of the tables it 
can be omitted, but as we have said it is better to leave it in for clarity.
5. The name of the second table is given in the `join` clause.
6. The conditions of the `join` are given in the `on` clause. The `on` clause is very much like a `where` clause, in that you specifiy expressions which restrict what rows
are output. In our example we have three expressions. The last two are the individual expressions we used in the previous, single table queries. The first 
expresiion `a.Id = b.Id` is the expression whih determines how we want the two tables to be joined. 
We are only interested in rows from both table where the `Id` values match.

When we run this query we get output like the following:

![Join1](../fig/SQL_09_join1.png)

> ## Exercise
>
> 1. The output includes the `Id` column from both tables, how could you have distinguished between them when you wrote the query?
> 2. Can you explain why there are two rows for `Id` 111?  Can you change the query so that these two different rows are being correctly displayed?
>
> > ## Solution
> > 
> > 
> > ~~~
> > select a.Id as Farms_Id, a.B_no_membrs,
> >        b.Id as Crops_Id, b.plot_Id, b.D_curr_crop
> > from Farms as a
> > join Crops as b
> > on a.Id = b.Id and a.B_no_membrs > 12 and b.D_curr_crop = 'maize'
> > ;
> > ~~~
> > {: .sql}
> > 
> > 1. we can add alias' to the two `Id` columns to distinguish them.
> > 2. by adding the `plot_Id` column from the Crops table. It is clear that `Id` 111 has two plots (plot 1 and plot 2) growing maize.
> > 
> > 
> {: .solution}
{: .challenge}



## Different join types

The example of a join given above is called an **INNER** join, we could have written **INNER JOIN** rather than 
simply **JOIN**. This is almost never done in practice as the inner join is by far the most common join type used.

Other Join types are available...

Before we look at the other join types we need to explain how the **Inner join** works and why it is so commonly used. 

For any join (type) we are defining a relationship between two tables based on the data values in two columns, one from each table. 
The relationship is given by the criteria in the `ON` clause. 
The value of the column in one table must be same as that in the other table. That is; the criteria is given in the form
`value_in_column_from_table_a = value_in_column_from_table_b`.
Only if this criteria is TRUE will the requested columns from table_a and table_b be returned as a single row in the output. 
During the join process each row of the first with every row of the second and if a match is found then a row combining the columns from both
tables is output. 

Although typically the values being matched from the first table are a unique (Distinct) set of values, the values in the second table don't have
to be unique. This is why in the results of our previous query there are two entries with Id 111. In the second table there are two records with Id 111 and so the record from the first table gets combined with both the records in 
the second table and two records are output.

Because every Farm grows some crops, there will be at least one record for each Id output. I for whatever reasonn the was a Farm with no crops 
then thare wou;ld be no record output for that Farm Id. Similarly if there was an entry in the Crops table with an Id which didn't match any of the Ids in the Farms table, 
then it would not be output. There is only an output record when the two columns have matching values.


When a relational database is defined and the tables set up initially the relationship between the tables are already known, 
they are part of the design of the overall database. Because of this it is possible to ensure when the data is added to the tables that there will be entries 
in both tables which have matching values. At the very least you can prevent rows being added to the second table with a value in the column you intend to join on for which there 
is no matching column in the first table.

An inner join only returns rows where there is a match between the two columns. In most cases this will be all of the columns selected from the first table and 0,1 or more columns selected from the second table. 

The relational design makes use of multiple tables as a way of avoiding repetition of data. Joining tables re-introduces the replication of the data. 


## There are several different join types possible

|Join Type | What it does |
|-----------------------------|:------|
|Inner Join | Matched rows in both tables are returned|
|Left outer join | All row in the left hand table are returned along with the matches from the right hand table or NULLs if there is no match|
|Right outer join	| All row in the right hand table are returned along with the matches from the left hand table or NULLs if there is no match|
|Full outer join	| All rows from both tables are returned, with NULLs where there are no matches|
|Cross join	| Each row in the first table will be matched with every row in the second table. It is possible to imagine situations where this is required but in most cases it is a mistake and un-intended. |

In SQLite only the `Inner join`, the `Left Outer join` and the `Cross join` are supported. You can create a `Right outer join` by swapping the tables in the `From` and `Join` clauses. A `Full outer join` is the combination of the Left outer and Right outer joins.
 
 

## Using different join types in analysing your data

In many cases the data you have in your tables may have come from disparate sources, in that they do not form part of a **planned** relational database. It has been 
your decision to bring together (join) the data in the tables. 

In order to do this at all you must be confident that the tables of data do have columns which have a common set of values that you can join on.

Assuming you do have a common column to join on, you can use an `Inner join` to combine the data.

However it will also be important for you to establish rows in both of the tables for which there is no matching row in the other table.

* You may expect some to be missing
* You may not care that some are missing
* You may need to explain why some are missing

To do this you will want to use a `Full outer join` or in the case of SQLite a `Left outer join` run twice using both tables in the `From` and `Join` clauses. We can demonstrate ability
`Left outer join` using the Crops_rice table we created ealier. 

The query below is similar to our original join except that we are now joining with the crops_rice table and we have dropped the additional criteria.

~~~
select a.Id as Farms_Id, a.B_no_membrs,
       b.Id as Crops_Id, b.D_curr_crop
from Farms as a
left outer join Crops_rice as b
on a.Id = b.Id 
~~~
{: .sql}

You can see from the results that there is an entry for every record in the Farms table, but unless there is a crop of rice, the entries in the columns from the crops_rice 
table are shown as NULL.

![Left Outer Join1](../fig/SQL_09_Left_Outer_Join_1.png)

### Joins with more than two tables

Joins are not restricted to just two tables. you can have any number, but the more you have the more unreadable the SQL query is likely to become. Quite often you can create views to hide this complexity.

Our original question was: 'Which Farms with more than 12 people in the household grow Maize?' We founfd the number of people in the household from the Farms table and the crops they grew in the crops table.
Suppose we now wanted to change the question to be: For Farms with more than 12 people in the hosehold how much land is devoted to growing Maize? In addition to the previous 
requiements we now also need the size of the plots growing maize. This information is only contained in the `plots` table. 
The `plots` table has both an Id column which we can use to join it with the Farms column. There is also a plot_Id column which is used to indicate the number of 
the plot within the Farm. The `crops` table also has a plot_id column used for the same purpose.

However we cannot join the `plots` and the `crops` table with just the the `plot_id` column becaquse the `plot_id` column is not unique within the the Plots table. The `plot_id` is the 
plot number within a Farm. So every Farm will have a plot_id with the value 1. In order to make what we join on unique we need to use both the Id column and the plot_id column together. This is allowed and quite 
commonly done.

Our new query now looks like this:

~~~
select a.Id as Farms_Id, a.B_no_membrs,
       b.Id , b.plot_id as plot_id, b.D02_total_plot,
       c.Id as Crops_Id, c.plot_Id as crops_plot_id, c.D_curr_crop
from Farms as a
join Plots as b
join Crops  as c
on a.Id = b.Id and ( b.Id = c.Id and b.plot_id = c.plot_id) and a.B_no_membrs > 12 and c.D_curr_crop = 'maize'
;
~~~
{: .sql}

Things to notice:
1.  There is a `join` clause for each of the additional tables
2. But there is only one `on` clause containing all of the needed criteria.
3. The two criteria in brackets represents the join of the `plots` table to the `Crops` table. (The brackets aren't needed, I just added them for clarity).

The results look like this:

![Left Outer Join1](../fig/SQL_09_join5.png )


> ## Exercise
>
> 1. Modify the query above so that only the 'Id', 'D02_total_plot' and the 'D_curr_crop' columns are shown and at the same time summarise the data so that there is only one entry for each Farm.
> i.e sum the 'D02_total_plot' column.
>
> 
> > ## Solution
> > 
> > 
> > ~~~
> > select a.Id as Farms_Id, 
> >        sum(b.D02_total_plot) as total_planted,
> >        c.D_curr_crop
> > from Farms as a
> > join Plots as b
> > join Crops  as c
> > on a.Id = b.Id and ( b.Id = c.Id and b.plot_id = c.plot_id) and a.B_no_membrs > 12 and c.D_curr_crop = 'maize'
> > group by a.Id, c.D_curr_crop
> > ;
> > ~~~
> > {: .sql}
> > 
> > 
> {: .solution}
{: .challenge}

