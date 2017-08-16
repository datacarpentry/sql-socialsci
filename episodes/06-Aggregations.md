---
title: "Aggregations"
teaching: 0
exercises: 0
questions:
- "How can I summarise the data in my tables"
objectives:
- "Use the Distinct keyword to get a unique set of values"

- "Usie the ‘group by’ clause to summarise data"

- "Use built-in statistical functions to provide column summaries"

- "Use the ‘having’ clause to provide selection criteria to the summary values"

- "Understand the difference between the ‘where’ and the ‘having’ clauses"

keypoints:
- "First key point."
---

## Using built-in statistical functions

Aggregate functions are used perform some kind of mathematical or statistical calculation across a group of rows. The rows in each group are determined by the different values in a specified column or columns.  Alternatively you can aggregate across the t
entire table.

If we wanted to know the minimum, average and maximum age values (numage) across the whole SN7577 table we could write a table such as this;

~~~ 
SELECT numage,
             min(numage),
             avg(numage),
             max(numage)
FROM SN7577
~~~ 
{: .sql}

This sort of query provides us with a general view of the values for a particular column or field across the whole table.

It is more likely that we would want to find such values for a range, or multiple ranges of rows where each range is determined by the values of some other column in the table.


## The `Distinct` keyword 

For the SN7577 table the **allowable** values in many of the columns are listed in the  [SN7577](../xxx/audit_of_political_engagement_11_ukda_data_dictionary.rtf) document. But this doesn't mean that they all actually appear in the data.

To obtain a list of umique values in a particular column you can use the `Distinct` keyword. We will use the text version of the SN7577 table for these examples. 

For Q1 `How would you vote if there were a General Election tomorrow?` there are 11 possibilities listed and another (-1) for a missing value. 

To find out which values are in the data we can use the query;

~~~ 
SELECT Distinct Q1
FROM SN7577_Text
~~~ 
{: .sql}

We can see from the reults of running this that all 11 values are represented and that there is no missing data in this field.

You can hve more than one column name after the `Distinct` keyword. In which case the results will include a row for each unique combination of the columns involved

> ## Exercise
>
> Q3 asks for the likelihood of voting on a scale of 1 - 10 (10 - Absolutely certain to vote).
> Write a quey which shows all of the combinations of voting intentions (Q1) and likelihood of voting (Q3).
> 
> > ## Solution
> > 
> > ~~~
> > SELECT Distinct Q1, Q3
> > FROM SN7577_Text
> > Order by Q1
> > 
> > ~~~
> > {: .sql}
> >
> > Sorting the results by Q1 gives the political parties in alphabetical order.
> {: .solution}
{: .challenge}


## The `group by` clause to summarise data

Just knowing the combinations is of limited use. Yo ureally want to know **How many** of of each of the values there are. 
To do this we use the the `Group By` clause.

~~~ 
SELECT Q1,
       count(*) as Num_potential_voters
FROM SN7577_Text
Group By Q1
Order by Q1
~~~ 
{: .sql}

This query gives us a count of potential voters for each party.

In much the same way as we did for the Distinct clause, we can group by more than one column.

~~~ 
SELECT Q1, 
       Q3,
       count(*) as Num_potential_voters
FROM SN7577_Text
Group By Q1, Q3
Order by Q1
~~~ 
{: .sql}


## Using the `having` clause 

