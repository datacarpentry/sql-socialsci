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

##About table joins

In any relational database system, the ability to join tables together is a key querying requirement.
Joins are used to combine the rows from two (or more) tables together to form a single table. A join between tables will only be possible if they have at least one column in common. The column doesn’t have to have the same name in each table, and quite often they won’t, but they do have to have a common usage.

For example the Q1 table has been created from the information about Q1 in the xxxx file. There are only two columns The vlue column indicates a respondents voting intentions and the key column has the value which is used to represent this intention in the SN7577 table.

You can see immediately from this that there is a connection or relationship between the the two tables. The key column in the Q1 table has been defined as a primary key. This guarantees that the key column has a unique set of values. Although not required, it is generally the case that the values in one of the columns that the tables have in common will have unique values in it. It is also not required but generally the case that the unique column will be a primary key in one of the tables.

In the SN7577 table, the values in the Q1 **column** 

Different join types

Using different join types in analysing your data

