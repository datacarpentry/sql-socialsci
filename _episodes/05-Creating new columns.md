---
title: "Introduction"
teaching: 15
exercises: 15
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

## Creating new columns

In addition to selecting existing columns from a table, you can also create new columns based on the exisiting columns. 

The SN7577 table has a series of 25 columns (daily1 - daily25) with values of 0 or 1 depending on whether or not a particular newspaper is read. If you want to know which newspaper is which, they are listed in the Newspapers table.

What we want to know is how many newspapers a given household (row in the SN7577 table) reads.

We could run the query below and count up the ones?

~~~ 
SELECT daily1, daily2, daily3, daily4, daily5, daily6, daily7, daily8,
       daily9, daily10, daily11, daily12, daily13, daily14, daily15,
       daily16, daily17, daily18, daily19, daily20, daily21, daily22,
       daily23, daily24, daily25
FROM SN7577;
~~~ 
{: .sql}

But it would be far more efficient to let the SQL query do that for us. So instead of selecting 25 columns, we are going to add the numeric values of the 25 columns into one new colum.
~~~ 
SELECT (daily1 + daily2 + daily3 + daily4 + daily5 + daily6 + daily7 + daily8 +
        daily9 + daily10 + daily11 + daily12 + daily13 + daily14 + daily15 +
        daily16 + daily17 + daily18 + daily19 + daily20 + daily21 + daily22 +
        daily23 + daily24 + daily25)
FROM SN7577;
~~~ 
{: .sql}

Running this query will give the correct answers, but it uses the expression used in creating the new column as the column name. This looks very messy, especially with such a long expression. It is always the case that if you create a column in the results of the query it won't have a name by default. SQL will create one for it. Other relational databases take different approaches to the problem and will pseudo-randomly name the new columns for you with such things as '_c0'. SQLite uses the expression you used to create the column name.

## Renaming columns using alias'

Given that creating new columns is so commonly done, SQL does provide a mechansim for giving them names of your choice. This is done using the **AS** clause

~~~ 
SELECT (daily1 + daily2 + daily3 + daily4 + daily5 + daily6 + daily7 + daily8 +
        daily9 + daily10 + daily11 + daily12 + daily13 + daily14 + daily15 +
        daily16 + daily17 + daily18 + daily19 + daily20 + daily21 + daily22 +
        daily23 + daily24 + daily25) AS Total_dailies
FROM SN7577;
~~~ 
{: .sql}

The **AS** keyword itself is optional. You can just put the name of the new column, but I think using the **AS** keyword adds clarity.
Creating column names in this way is reffered to as adding an alias. This may seem a bit strange for columns which had no real name in the first place, but the point is, you can give any table column name an alias to be used in the output rather than the original. 

In the SN7577 table the column names of 'Q1', 'Q5aiv', etc. are pretty meaningless. You may well decide to change such names with alias'.

## Using built-in functions to create new values
In addition to using simple arithmetic operations to create new columns, you can also use some of the SQLite builtin functions. Full details of the available builtin functions are available from the SQLite.org wbsite [here](https://sqlite.org/lang_corefunc.html#instr).

We will look at some of the arithmetic and statistical functions when we deal with aggregations in a later lesson. For now we will focus on some text functions. 

To do this we will use the SN7577_Text table. This table has the same information in it as the SN7577 table but many of the numeric values have been replaced with their text equivalents. To find out how these text equivalents map to the numeric values you need to refer to the SN7577 data dictionary document which can be downloaded from [here](../data/audit_of_political_engagement_11_ukda_data_dictionary.docx).

> ## Exercise
>
> From the Browse Data tab, select the SN7577_text table from the drop down list at the top.
> Notice that a lot of the coluns now contain text, but some are still numbers and some have both numbers and text.
> Apart from the key_id column at the beginning, all of the columns are considered to be text.
>
{: .challenge}

There will be some circumstances where a text 'number' will cause problems, like in arithmetic. We can avoid such problems by using the `cast` function. This tells SQLite to change the data type of a data item. It is most commonly used in the way we will use it in changing a text string into an integer or real value. 

The format of the `cast` function is shown in the following example.

~~~ 
SELECT numage,
       cast(numage as Integer)
FROM SN7577_Text;
~~~ 
{: .sql}

> ## Exercise
>
> Run the example above in the Execute SQL tab. Notice that there is no way of telling that the data type has changed. DB Browser does not right align numbers as you might expect.
>
{: .challenge}

We will now look at a couple of the more common text functions. These have equivalents in other programming languages or spreadsheet systems, sometimes with different names.

| SQLite function  | Excel equivalent |
|------------------|:-----------------|
| substr(a,b,c)    | mid(a,b,c)       |
| instr(a,b)       | find(a,b)        |

The question in Q5axv asks whether or not you have influenced policies in the last 12 months and required a boolean response yes or no. In the SN7577 table the responses are recorded as 0 and 1 and in the SN7577_Text file they are recorded as 'no null' and 'null'.

We want to write queries which will create a new column representing the SN7577 values from the SN7577_Text values. We can do this using either the `substr` or the `instr` function. The example below shows the use of `substr`

~~~ 
SELECT Q5axv,
   NOT (substr(Q5axv, 1,2) = "no" ) as Q5axv_value
FROM SN7577_Text;
~~~ 
{: .sql}

**Explanation**  
The substr function takes 2 characters starting at character 1 from the value in Q5axv. This is compare with the string 'no'. The result of the comparison is either the boolean value `True` or the boolean value `False`. SQLite represents the boolean values `True` and `False` as 1 and 0. However as we want to return 0 if the expression is True we need to NOT the whole expression.

> ## Exercise
>
>  1. Try running the query above and check the results.
>  2. Change the query to use the `instr` function and check that you get the same results.
> 
> > ## Solution
> > 
> > ~~~ 
> > SELECT Q5axv,
> >    NOT (instr(Q5axv, "no")) as Q5axv_value
> > FROM SN7577_Text;
> > ~~~ 
> > {: .sql}
> > 
> {: .solution}
{: .challenge}


## Using SQL syntax to conditionally create new values

The SQLite SQL dialect has a progeramming construct called the `case` statement. This is in fact common to many other programming languages as well. It can be used in two different ways, both of which can be used to create new columns in a queries' result set. 

First we will use the case statement to accomplish what we just did with the `substr` and `instr` functions.

~~~ 
SELECT Q5axv ,
       case Q5axv
            when "no null" then 0
            when "null" then 1
       end as Q5axv_bool 
FROM SN7577_Text;
~~~ 
{: .sql}

This format of the case statement allows you to check if various values **are equal** to the value in the field given after the `case` keyword.
There is a more general form which alows to to perform any kind of test.

## Using SQL syntax to create ‘binned’ values

It is often the case that we wish to convert a continous variable into a an discrete factor type variable. In SN7577 you can see this being done in the `numage` and the `age` variables. 

We can use a `case` statement to create this type of effect. The example below re-creates the `age` values used in the SN7577_Text table with the addition of an "Under age" category for those less than 18 years old.

~~~ 
SELECT numage ,
               case 
                   when numage between 18 and 24 then "18 - 24"
                   when numage between 25 and 34 then "25 - 34"
                   when numage between 35 and 44 then "35 - 44"
                   when numage between 45 and 54 then "45 - 54"
                   when numage between 55 and 59 then "55 - 59"
                   when numage between 60 and 64 then "60 - 64"
                   when numage          >= 65              then "65+"
                else
                    "Under age"
               end as numage_range
FROM SN7577;
~~~ 
{: .sql}

> ## Exercise
>
>  1. Try running the query above and check the results.
>  2. Are there any "Under age" entries?
> 
> > ## Solution
> > 
> > You can look for "Under age" entries by scanning down the results.
> >
> > A better way would be to adding  **ORDER BY numage** at the end of the query and then they will appear at the top of the results.
> >
> > You could also have written a seperate query **Select numage from SN7577 where numage < 18**
> > 
> {: .solution}
{: .challenge}
