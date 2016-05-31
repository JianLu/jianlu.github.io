---
layout: post
title: "Vertica Flex Table"
date: 2016-05-24 11:14:51
description: Introduction to Vertica Flex Table
categories: [Tech]
tags: [vertica, database]
---

* TOC
{:toc}

# Introduction

*Flex Table* was firstly introduced in Vertica 7.0 as *Flex Zone*,
which can load, parse and view semi-structured or un-structured data.
We installed Vertica 7.2 and created Flex Table from un-structured raw data.

## Flexible Data

Flex tables can contain only unstructured data in the format of key-value pair.
You also can add real columns for columnar data,
then you get a hybrid flex table consisting of both unstructured data and real columns.

You can create a flex table using:

~~~ sql
-- Create a flex table with only unstructured data
CREATE FLEX TABLE flext_unstructure();
~~~

Then check the fields of the new created flex table by:

~~~
\d flex_unstructured;
~~~

From the result below, each flex table row is in the pattern of key-raw pair.
If you create the table without other column definitions,
the numeric field `__identity__` is created as an auto-incrementing `IDENTITY(1,1)` column.
The field `__raw__` is a `LONG VARBINARY` column with a `NOT NULL` constraint
that contains the actual unstructured raw data.

~~~
                                                   List of Fields by Tables
 Schema |       Table       |    Column    |          Type          |  Size  | Default | Not Null | Primary Key | Foreign Key
--------+-------------------+--------------+------------------------+--------+---------+----------+-------------+-------------
 public | flex_unstructured | __identity__ | int                    |      8 |         | t        | f           |
 public | flex_unstructured | __raw__      | long varbinary(130000) | 130000 |         | t        | f           |
(2 rows)
~~~

You can also create a flex table with column definitions:

~~~ sql
CREATE FLEX TABLE flex_semiconstructured (
	name varchar PRIMARY KEY,
	time TIMESTAMP NOT NULL
);
~~~

~~~
\d flex_semiconstructured;
                                                   List of Fields by Tables
 Schema |         Table          | Column  |          Type          |  Size  | Default | Not Null | Primary Key | Foreign Key
--------+------------------------+---------+------------------------+--------+---------+----------+-------------+-------------
 public | flex_semiconstructured | __raw__ | long varbinary(130000) | 130000 |         | t        | f           |
 public | flex_semiconstructured | name    | varchar(80)            |     80 |         | t        | t           |
 public | flex_semiconstructured | "time"  | timestamp              |      8 |         | t        | f           |
(3 rows)
~~~

When the table exists, you can add new columns, like:

~~~ sql
ALTER TABLE flex_semiconstructured ADD column "category" VARCHAR;
~~~