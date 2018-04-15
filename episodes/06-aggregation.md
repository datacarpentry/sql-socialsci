---
title: "Aggregations"
teaching: 10
exercises: 10
questions:
- "How can I summarise the data in my tables"
objectives:
- "Use the Distinct keyword to get a unique set of values"
- "Usie the ‘group by’ clause to summarise data"
- "Use built-in statistical functions to provide column summaries"
- "Use the ‘having’ clause to provide selection criteria to the summary values"
- "Understand the difference between the ‘where’ and the ‘having’ clauses"
keypoints:
- "Builtin functions can be used to produce a variety of summary statistics"
- "The `DISTINCT` keyword can be used to find the unique set of values in a column or columns"
- "Data in columns can be summarised by values using the `GROUP BY` clause"
- "Summarised data can be filtered using the `HAVING` clause"
---

## Using built-in statistical functions

Aggregate functions are used perform some kind of mathematical or statistical calculation across a group of rows. The rows in each group are determined by the different values in a specified column or columns.  Alternatively you can aggregate across the entire table.

If we wanted to know the minimum, average and maximum age values (numage) across the whole SN7577 table we could write a query such as this;

~~~ 
SELECT 
    min(numage),
    avg(numage),
    max(numage)
FROM SN7577;
~~~ 
{: .sql}

This sort of query provides us with a general view of the values for a particular column or field across the whole table.
  
`min` , `max` and `avg` are builtin aggregate functions in SQLite (and any other SQL database system). There are other such functions avaialable. A complete list can be found in the SQLite documentation [here](https://sqlite.org/lang_aggfunc.html)  
   
It is more likely that we would want to find such values for a range, or multiple ranges of rows where each range is determined by the values of some other column in the table. Before we do this we will look at how we can find out what different values are contained in a given column.


## The `Distinct` keyword 

For the SN7577 table the **allowable** values in many of the columns are listed in the  [SN7577](../xxx/audit_of_political_engagement_11_ukda_data_dictionary.rtf) data dictionary document. But this doesn't mean that they all actually appear in the data.

To obtain a list of umique values in a particular column you can use the `Distinct` keyword. We will use the text version of the SN7577 table for these examples. 

For Q1 `How would you vote if there were a General Election tomorrow?` there are 11 possibilities listed and potentially another (-1) for a missing value. 

To find out which values are in the data we can use the query;

~~~ 
SELECT DISTINCT Q1
FROM SN7577_Text;
~~~ 
{: .sql}

We can see from the reults of running this that all 11 values are represented and that there is no missing data in this field.

You can have more than one column name after the `Distinct` keyword. In which case the results will include a row for each unique **combination** of the columns involved

> ## Exercise
>
> Q3 asks for the likelihood of voting on a scale of 1 - 10 (10 - Absolutely certain to vote).
> Write a query which shows all of the combinations of voting intentions (Q1) and likelihood of voting (Q3).
> 
> > ## Solution
> > 
> > ~~~
> > SELECT DISTINCT Q1, Q3
> > FROM SN7577_Text
> > ORDER BY Q1;
> > 
> > ~~~
> > {: .sql}
> >
> > Sorting the results by Q1 gives the political parties in alphabetical order.
> {: .solution}
{: .challenge}


## The `group by` clause to summarise data

Just knowing the combinations is of limited use. You really want to know **How many** of each of the values there are. 
To do this we use  the `GROUP BY` clause.

~~~ 
SELECT Q1,
       count(*) as Num_potential_voters
FROM SN7577_Text
GROUP BY Q1
ORDER BY Q1;
~~~ 
{: .sql}

This query gives us a count of potential voters for each party.

In the first example of this episode, three aggregations were performed over the single column Q1. In addition to calculating multiple aggregation values over a single column, it is also possible to aggregate over multiple columns by specifying them in all in the `SELECT` clause **and** the `GROUP BY` clause. 

The grouping will take place based on the order of the columns listed in the `GROUP BY` clause. There will be one row returned for each unique combination of the columns mentioned in the `GROUP BY` clause

What is not allowed is specifying a non-aggregated column in the select clause which is not mentioned in the group by clause.

~~~ 
SELECT Q1, 
       Q3,
       count(*) as Num_potential_voters
FROM SN7577_Text
GROUP BY Q1, Q3
ORDER BY Q1;
~~~ 
{: .sql}

> ## Exercise
> In fact SQLite, unlike other RDBMS systems, will allow you to specify columns in the `SELECT` clause which are not in the `GROUP BY` clause. 
> Repeat the query above but add an extra column name into the `SELECT` clause.
>
> What results are returned? 
> Can you explain them? (You may want to browse the SN7577_text table in the Browse data tab.
>
> > ## Solution
> >
> > ~~~
> > SELECT Q1, 
> >        Q3,
> >        Q4,
> >        count(*) as Num_potential_voters
> > FROM SN7577_Text
> > GROUP BY Q1, Q3
> > ORDER BY Q1;
> > ~~~
> > 
> > The same number of rows will be returned with the same values for Q1 and Q3. The cvalues for Q4 are the last value in the 
> > table for the given combination of Q1 and Q3.
> > 
> > It is unlikely that this is what was intended.
> > 
> {: .solution}
{: .challenge}


## Using the `having` clause 

In order to filter the rows returned in a non-aggregated query we used the `WHERE` clause. For an aggregated query the equivalent is the 'HAVING` clause.

You use the 'HAVING` clause by providing it with a filter expression which references one or more of the aggregated columns. 

In a `HAVING` clause you can use the column alias to refer to the aggregated column.

~~~ 
SELECT Q1 ,
       sum(daily3) as Telegraph_reader
FROM SN7577 
GROUP BY Q1
HAVING Telegraph_reader > 5;
~~~ 
{: .sql}

In this example aggregating 'Telegraph' readers by voter intentions (Q1).

We are only interested in the groups where there are more than 5 'Telegraph' readers.

> ## Exercise
>
> In the UK the 'Telegraph' is regarded as a right-wing newspaper and the Conservatives are considered to be a right-wing political party.
> 
> Do the results of the query above support this?
>
> The 'Mirror' is a left-wing newspaper and Labour are considered to be a left-wing political party. 
> Re-write the query above to see which group of supporters are more likely to read the Mirror 
> 
> You can browse the Newspapers table to find out which of the daily columns refers to the Mirror.
> You can browse the Question1 table to find the party names represented by the values in Q!
> 
> > ## Solution
> > 
> > ~~~
> > SELECT Q1 ,
> >        sum(daily12) as Mirror_reader
> > FROM SN7577 
> > GROUP BY Q1
> > HAVING Mirror_reader > 5;
> > ~~~
> > {: .sql}
> >
> > You can see from the results and the original query that no Conservative supporters who read the Mirror and no Labour supporters who read the Telegraph.
> {: .solution}
{: .challenge}

 
