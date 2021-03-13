---
title: 'Spark: Now It''s Real Big Data'
date: 2021-03-09 13:17:23
author: FadingWinds
img:
categories:
tags:
	- Udemy
	- notes
	- Scala
	- Python
	- updating
summary: Notes for Learning Spark with Scala and Spark with Python on Udemy.
top:
hidden:
password:
---

Ever since I started to study information systems / information science, the term "big data" has long been used to explain what my major is doing, but the actual entity it represents is hardly ever reachable, at least to me.

Now I'm learning Spark, and hopefully after this I don't have to feel guilty for saying "big data" as if it's something I know more than "that's what Excel couldn't store" (a professor once said this during a class) any more.

*Most notes are based on Udemy course: ![Apache Spark with Scala: Hands on with Big Data](https://www.udemy.com/course/apache-spark-with-scala-hands-on-with-big-data/)*

### Overview

- Spark is 100x faster in memory and 10x faster on disk compared with Hadoop MapReduce because of its DAG (directed acyclic graph) engine. Still, Spark and Hadoop can co-exist and not necessarily a substitute for each other.
- Using Spark Datasets and DataFrames is similar to using SQL.
- Why Scala?
	- Spark is written in Scala.
	- Scala forces you to use functional programming (a good fit for distributed processing).
	- (Now only a little) faster than Python, more simple than Java.
- Spark 3 can integrate with TensorFlow to do deep learning.
- Scala runs on JVM.

### Scala Basics

#### Syntax

*Just refer to the file `LearningScala*.sc`(* = {1 ,2 ,3, 4}) and you should be fine.*

- `val`s are immutable, in contrast to `var`s. Using immutable constants as much as possible to achieve thread safety and avoid race conditions.
- Different prefix (e.g. "s") can allow you to e.g. use variables and expressions in the `println("")` string.
- Regular expression: use triple parentheses and add `.r` at the end.
- No ";" in Scala!
- `_` is a catch-all symbol in "match-case" expressions.
- For loop in Scala is not like Java or Python...or even a mix of them.
- **Having an expression in Scala will give an output of the result of the last thing.**
- Functions syntax in Scala is similar to Python (sort of), with curly brackets and an extra `=`.
- **Indexing**
	- **Tuple index starts with 1, writen as `tuple._1` for getting the first element.**
	- **List index starts with 0: `list(0)`**
- `list.head` gives the first element, but **`list.tail` gives all the remaining elements except the first one**.
- Concatenate list: `listA ++ listB`.
- Add element to list: `x :: list`.
- **If there's no `return` in the function, the last expression will be taken as the return value.**
- `object` is like a `class`, but there's also a `case class` which used in DataSets to define a schema first.

#### Functional Programming

Similar to what I learned on *Machine Learning Engineering* class, functional programming in Scala looks like this:

```
def transformInt(x: Int, f: Int => Int) Int = {f(x)}
```

#### Lambda Expression

Similar to that in Java and Python:

`transformInt(3, x => x * x * x)`

Or:

```
val ex1 = (x:Int) => x + 2
println(ex1(7)) \\ output: 9
```

#### Data Structures

- Tuples can have different data typles, but lists can't.
- Functions like Map, Reduce, Filter are good for being parallelizable. (Spark has its own Map, Reduce Filter other than Scala's)
- Maps: dictionaries in Scala. A special function is `utils.Try(map("x")) getOrElse "Unknown"`
- In Java terms, Scala's `Seq` would be Java's `List`, and Scala's `List` would be Java's `LinkedList`. (Ref: ![Stack Overflow](https://stackoverflow.com/questions/10866639/difference-between-a-seq-and-a-list-in-scala))

#### Built-in Functions

- String related functions, e.g. `string.toUpperCase`, `string.reverse`
- Reduce: `val sum = numberList.reduce( (x: Int, y: Int) => x + y)`
- Filter: `val iHateThrees = numberList.filter(_ != 3)`
- List related functions: `list.reverse`, `list.sorted`, `list.max`, `list.sum`, `list.distinct`(producing a set), `list.contains()`(returns a boolean)


### RDD (Resilient Distributed Dataset)

RDD is a lower level API. Seems like it's getting less popular.

Rows of data can be divided and sent to different computer to process in parallel.

RDDs can be created from text file, hadoop(hdfs), hive context, sql commands, etc.

Transforming RDDs:

- map and flatMap (1 **row** to multiple **rows**)
- filter
- distinct
- sample
- union, intersection, subtract, cartesian

RDD common actions:

- Collect
- Count
- countByValue
- take
- top
- reduce

**Nothing actually happens in your driver program until an action is called.** ~~Which can lead to tricky debuggings~~:raised_eyebrow:.

SparkContext(from Doc): Main entry point for Spark functionality. A SparkContext represents the connection to a Spark cluster, and can be used to create RDDs, accumulators and broadcast variables on that cluster.

Only one SparkContext should be active per JVM. 

Process: Job -> Stages (reorganized data will be a new stage) -> Tasks (distributed across cluster, scheduled and executed)

**Key/Value RDD Specific Functions**

- reduceByKey(): the (x, y) in example "FriendsByAge" is actually representing two rows (entries).
- groupByKey()
- sortByKey()
- keys(), values()
- SQL style joins
- mapValues() / flatMapValues()

### SparkSQL

DataFrames:

- Contains raw objects
- Can run SQL queries
- Has a schema
- R/W to JSON, Hive, etc.
- Communicates with JDBC/ODBC, Tableau

Technically, a DataFrame is just a DataSet with raw objects. DataSets wraps a given struct or type. 
DataSets will know its schema at compile time (Python can't do this, only have DataFrames).

RDD can be converted to DataSets and vice versa.

When using SparkSQL, create SparkSession instead of SparkContext, and need to stop the session when finish.

DataSets have similar functions to SQL queries in which it doesn't need a database view to execute. (Personally I think it's easier to use than passing SQL commands, ~~definitely not because I've forgotten so much about SQL, ~~ except for the weird syntax `=!=` and `===` in `.filter()`)

DataSets work best with structured data.

For loading unstructured data, the default column name should be set to "value" for the schema to be inferred.

\*Default `show()` will display 20 rows and truncate. Use `show(NUM OF ROWS, false)`.

For files with no headers, schema can't be inferred so a new schema need to be defined first: `val schema = new StructType.add("COLUMN NAME", TYPE)`