---
title: "Introduction"
teaching: 0
exercises: 0
questions:
- "How can I add new columns with derived values in the query results?"
- "How can I give a column a new name?"
- "How do I use built-in functions to create new values?"
- "How can I create binned results?"
- "How can I add new columns with derived values in the query results?"
- "How can I give a column a new name?"
- "How do I use built-in functions to create new values?"
- "How can I create binned results?"

objectives:
- "Create new columns in the query output"
- "Rename columns in the query output"
- "Use built-in functions to create new values"
- "Use SQL syntax to conditionally create new values"
- "Use SQL syntax to create a new column of ‘binned’ values"

keypoints:
- "First key point."
---

# Lesson

## Creating new columns

In addition to selecting existing columns from a table, you can also create new columns based on the exisiting columns. 

The SN7577 table has a series of 25 columns (daily1 - daily25) with values of 0 or 1 depending on whether or not a particular newspaper is read. If you want to know which newspaper is which, they are listed in the Newspaper table.

What we want to know is how many newspapers a given household (row in the SN7577 table) reads.

We could run the query below and count up the ones?

~~~ 
SELECT daily1, daily2, daily3, daily4, daily5, daily6, daily7, daily8,
       daily9, daily10, daily11, daily12, daily13, daily14, daily15,
       daily16, daily17, daily18, daily19, daily20, daily21, daily22,
       daily23, daily24, daily25
FROM SN7577
~~~ 
{: .sql}

But it would be far more efficient to let the SQL query do that for us. So instead of selecting 25 columns, we are going to add the numeric values of the 25 columns into one new colum.
~~~ 
SELECT (daily1 + daily2 + daily3 + daily4 + daily5 + daily6 + daily7 + daily8 +
        daily9 + daily10 + daily11 + daily12 + daily13 + daily14 + daily15 +
        daily16 + daily17 + daily18 + daily19 + daily20 + daily21 + daily22 +
        daily23 + daily24 + daily25)
FROM SN7577
~~~ 
{: .sql}

Running this query will give the correct answers, but it uses the expression used in creating the new column as the column name. This looks very messy, especially with such a long expression. It is always the case that if you create a column in the results of the query it won't have a name by default. SQL will create one for it. Other relational databases take different approaches to the problem and will pseudo-randomly name the new columns for you with such things as '_c0'. SQLite uses the expression you used to create the column.

## Renaming columns using alias'

Given that creating new columns is so commonly done, SQL does provide a mechansim for giving them names of your choice. This is done using the **AS** clause


~~~ 
SELECT (daily1 + daily2 + daily3 + daily4 + daily5 + daily6 + daily7 + daily8 +
        daily9 + daily10 + daily11 + daily12 + daily13 + daily14 + daily15 +
        daily16 + daily17 + daily18 + daily19 + daily20 + daily21 + daily22 +
        daily23 + daily24 + daily25) AS Total_dailies
FROM SN7577
~~~ 
{: .sql}

The **AS** keyword itself is optional. You can just put the name of the new column, but I think using the **AS** keyword adds clarity.
Creating column names in this way is reffered to as adding an alias. This may seem a bit strange for columns which had no real name in the first place, but the point is, you can give any table column name an alias to be used in the output rather than the original. 

In the SN7577 table the column names of 'Q1', 'Q5aiv', etc. are pretty meaningless. You may well decide to change such names with alias'.

## Using built-in functions to create new values
In addition to using simple arithmetic operations to create new columns, you can also use some of the SQLite builtin functions. Full details of the available builtin functions are available from the SQLite.org wbsite [here](https://sqlite.org/lang_corefunc.html#instr).

We will look at some of the arithmeticc and statistyical function when we deal with aggregations in a later lesson. For now we will focus on some text functions. 

To do this we will use the SN7577_Text table. This table has the same information in it as the SN7577 table but many of the numeric values have been replaced with their text equivalents. To find out how these text equivalents map to the numeric values you need to refer to the SN7577 data dictionary document which can be downloaded from [here - don't know what this link will be]

> ## Exercise
>
> Select the SN75&& table in the left pane of th plugin and in the right pane select the `Browse & Search` tab.  
> Notice that a lot of the coluns now contain text, but some are still numbers and some have both numbers and text.
> Apart from the key_id column at the beginning, all of the columns are considered to be text, you can tell by the light blue colouring.
>
{: .challenge}

There will be some circumstances where a text 'number' will cause problems, like in arithmetic. we can avid such problems by using the `cast` function. This tells SQLite to change the data type of a data item. It is most commonly in the way we will use it in changing a text string into an integer or real value. 

The format of the `cast` function is shown in the following example.

~~~ 
SELECT numage,
       cast(numage as Integer)
FROM SN7577_Text
~~~ 
{: .sql}

> ## Exercise
>
> Run the example above in the plugin. Notice that the only way of telling that the data type has changed is by the colour coding. The plugin does not right align numbers as you might expect.
>
{: .challenge}

We will now look at some of the more common text functions. These all have equeivalents in other programming languages or spreadsheet systems, sometimes with different names.



## Using SQL syntax to conditionally create new values

## Using SQL syntax to create ‘binned’ values
