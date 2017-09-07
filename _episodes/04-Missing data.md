---
title: "Introduction"
teaching: 0
exercises: 0
questions:
- "How can I deal with missing data?"
objectives:
- "Recognise what the database sees as missing data"

- "Understand that the original data source may represent missing data differently"

- "Define strategies for dealing with missing data"

keypoints:
- "First key point."
---

## How does the database represents missing data

At the beginning of this lesson we noted that all database system have the concept of a NULL value. Something which is missing and nothing is known about it.

In the SQLite plugin, a table which has NULL values has the cell displayed in **pink**. The example below is a version of the SN7577 table with most of the columns removed and some NULL values introduced in the `numage` column.

![SN7577_nulls](../fig/SQL_04_Nulls_01.png)

This table was created from a csv file which looks like this

![SN7577_nulls_csv](../fig/SQL_04_Nulls_02.png)

The line numbers on the left are from the text editor and are not part of the data.

You can see that in lines 4, 9 and 15 there are consequetive ',' where the numage values should be. These values are missing from the data

## Other representations of missing data  

## Dealing with missing data
