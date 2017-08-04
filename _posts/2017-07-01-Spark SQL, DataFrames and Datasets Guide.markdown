---
layout:     post
title:      "spark2.2.0"
subtitle:   "Spark SQL, DataFrames and Datasets Guide"
date:       2017-08-01 09:00:00
author:     "Johnnwen"
header-img: "img/spark2.2.0.png"
catalog:    true
tags:
    - 大数据
    - spark  
    
---

# Spark SQL, DataFrames and Datasets Guide

## 概要

* Spark SQL是用于结构化数据处理的Spark模块，与基本的Spark RDD API不同， Spark SQL API可以为结构化数据提供更多的信息。在底层，Spark SQL使用这些额外的信息进行进一步优化执行过程。有几种与Spark SQL进行交互的方法，主要包括SQL和Dataset API。当计算一个结果时，可以使用相同的执行引擎，无论你使用哪一种语言或API。这意味着开发人员可以轻松地在不同API之间来回切换，基于此Spark SQL提供了最自然的方式来表达给定的转换。
   
## SQL

* Spark SQL的一个用途是执行SQL查询，也可用于从现有的Hive数据仓库中读取数据。当使用编程语言运行SQL时，其结果将作为Dataset / DataFrame返回。 还可以使用命令行或JDBC / ODBC与Spark SQL接口进行交互。

## Datasets and DataFrames

* Datasets是分布式数据集合，其是Spark 1.6版本中增加的一个新接口，不仅提供了RDD的优点（拥有强类型、强大的lambda功能），并具有Spark SQL优化执行引擎的优点。Dataset可以JVM对象构建，然后使用功能转换函数（map，flatMap，filter等）进行操作，Datasets API可用Scala和Java语言进行调用，Python不支持Dataset API。但是由于Python的动态性质，Dataset API的许多优点已经可用，可以通过row.columnName自然地的访相应的字段。

* DataFrames 是一个组织成命名列的数据集，它在概念上等同于关系数据库中的表或R / Python中的data frame,但是在更加优化的引擎下，DataFrames可以从各种各样的源构建，例如：结构化数据文件，Hive中的表，外部数据库或现有RDD，DataFrame API可以在Scala，Java，Python和R中使用。在Scala和Java中，DataFrame由 Dataset of Rows表示，而在Java API中，需要使用Dataset\<Row\>来表示DataFrame。

## 代码演示

### 1. Starting Point: SparkSession

Spark中所有功能的入口点是SparkSession类。 要创建基本的SparkSession，只需使用SparkSession.builder()方法。

```
import org.apache.spark.sql.SparkSession

val spark = SparkSession
  .builder()
  .appName("Spark SQL basic example")
  .config("spark.some.config.option", "some-value")
  .getOrCreate()

// For implicit conversions like converting RDDs to DataFrames
import spark.implicits._

```

***Spark 2.0中的SparkSession为Hive功能提供内置支持，包括使用HiveQL编写查询，访问Hive UDF以及从Hive表读取数据的功能。 要使用这些功能，并不需要有一个现有的Hive设置。***

### 2. Creating DataFrames

使用SparkSession，应用程序可以从现有的RDD，Hive表或Spark数据源创建DataFrames。作为示例，以下将基于JSON文件的内容创建DataFrame：

```
val df = spark.read.json("examples/src/main/resources/people.json")

// Displays the content of the DataFrame to stdout
df.show()
// +----+-------+
// | age|   name|
// +----+-------+
// |null|Michael|
// |  30|   Andy|
// |  19| Justin|
// +----+-------+

```

### 3. Untyped Dataset Operations (DataFrame Operations)

DataFrames为Scala，Java，Python和R中的结构化数据操作提供了一种特定领域的语言。

如上所述，在Spark 2.0中，DataFrames只是Scala中的Rows Dataset和Java API。与“强类型化转换”相反，这些操作也称为“无类型转换”，并带有强类型的Scala/Java数据集。

下面是使用Datasets进行结构化数据处理的一些基本示例：

```
// This import is needed to use the $-notation
import spark.implicits._
// Print the schema in a tree format
df.printSchema()
// root
// |-- age: long (nullable = true)
// |-- name: string (nullable = true)

// Select only the "name" column
df.select("name").show()
// +-------+
// |   name|
// +-------+
// |Michael|
// |   Andy|
// | Justin|
// +-------+

// Select everybody, but increment the age by 1
df.select($"name", $"age" + 1).show()
// +-------+---------+
// |   name|(age + 1)|
// +-------+---------+
// |Michael|     null|
// |   Andy|       31|
// | Justin|       20|
// +-------+---------+

// Select people older than 21
df.filter($"age" > 21).show()
// +---+----+
// |age|name|
// +---+----+
// | 30|Andy|
// +---+----+

// Count people by age
df.groupBy("age").count().show()
// +----+-----+
// | age|count|
// +----+-----+
// |  19|    1|
// |null|    1|
// |  30|    1|
// +----+-----+

```

***更多详情操作，请参阅[API文档](http://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.Dataset)。***

***除了简单的列引用和表达式，Datasets还具有丰富的函数库，包括字符串操作、日期运算、常用的数学运算等。 详情参考[DataFrame Function Reference](http://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.functions$)。***


### 4. Running SQL Queries Programmatically

SparkSession上的sql函数使应用程序以编程方式运行SQL查询，并将结果作为DataFrame返回。

```

// Register the DataFrame as a SQL temporary view
df.createOrReplaceTempView("people")

val sqlDF = spark.sql("SELECT * FROM people")
sqlDF.show()
// +----+-------+
// | age|   name|
// +----+-------+
// |null|Michael|
// |  30|   Andy|
// |  19| Justin|
// +----+-------+

```

### 5. Global Temporary View

Spark SQL中的临时视图是会话范围的，如果创建它的会话终止，它将消失。 如果要在所有会话之间共享临时视图，并保持活动状态，直到Spark应用程序终止，您可以创建一个全局临时视图。 全局临时视图与系统保留的数据库global_temp相关联，我们必须使用限定名称来引用它。 SELECT * FROM global\_temp.view1。

```
// Register the DataFrame as a global temporary view
df.createGlobalTempView("people")

// Global temporary view is tied to a system preserved database `global_temp`
spark.sql("SELECT * FROM global_temp.people").show()
// +----+-------+
// | age|   name|
// +----+-------+
// |null|Michael|
// |  30|   Andy|
// |  19| Justin|
// +----+-------+

// Global temporary view is cross-session
spark.newSession().sql("SELECT * FROM global_temp.people").show()
// +----+-------+
// | age|   name|
// +----+-------+
// |null|Michael|
// |  30|   Andy|
// |  19| Justin|
// +----+-------+

```

### 6. Creating Datasets

Datasets与RDD类似，但是，Datasets不是使用Java序列化或Kryo，其使用专门的编码器来序列化对象，从而进行网络处理或传输。 虽然编码器和标准序列化都将负责将对象转换成字节，但编码器是动态生成的代码，并允许Spark执行许多操作（如过滤，排序和散列），而不需要将将字节反序列化到对象中。

```
// Note: Case classes in Scala 2.10 can support only up to 22 fields. To work around this limit,
// you can use custom classes that implement the Product interface
case class Person(name: String, age: Long)

// Encoders are created for case classes
val caseClassDS = Seq(Person("Andy", 32)).toDS()
caseClassDS.show()
// +----+---+
// |name|age|
// +----+---+
// |Andy| 32|
// +----+---+

// Encoders for most common types are automatically provided by importing spark.implicits._
val primitiveDS = Seq(1, 2, 3).toDS()
primitiveDS.map(_ + 1).collect() // Returns: Array(2, 3, 4)

// DataFrames can be converted to a Dataset by providing a class. Mapping will be done by name
val path = "examples/src/main/resources/people.json"
val peopleDS = spark.read.json(path).as[Person]
peopleDS.show()
// +----+-------+
// | age|   name|
// +----+-------+
// |null|Michael|
// |  30|   Andy|
// |  19| Justin|
// +----+-------+
```

### 7. Interoperating with RDDs

Spark SQL支持将现有RDD转换为Datasets的两种不同方法。 第一种方法使用反射来推断包含特定类型对象的RDD的模式。 当编写Spark应用程序时，如果已经知道了RDD的模式，这种基于反射的方法会导致更简洁的代码，并且运行良好。

创建Datasets的第二种方法是通过编程接口，可以先构建一个模式，然后将其应用到现有的RDD。 虽然此方法更复杂，但它允许在运行时不知道列及其类型时构造Datasets。

#### 7.1 Inferring the Schema Using Reflection

Spark SQL的Scala接口支持自动将包含Case classes的RDD转换为DataFrame。Case classes定义了表的模式。使用反射来读取Case classes的参数名称，并成为列的名称。Case classes也可以嵌套或包含复杂类型，如Seq或Arrays。此RDD可以隐式转换为DataFrame，然后注册为表。该表可用于后续SQL语句。

```
// For implicit conversions from RDDs to DataFrames
import spark.implicits._

// Create an RDD of Person objects from a text file, convert it to a Dataframe
val peopleDF = spark.sparkContext
  .textFile("examples/src/main/resources/people.txt")
  .map(_.split(","))
  .map(attributes => Person(attributes(0), attributes(1).trim.toInt))
  .toDF()
// Register the DataFrame as a temporary view
peopleDF.createOrReplaceTempView("people")

// SQL statements can be run by using the sql methods provided by Spark
val teenagersDF = spark.sql("SELECT name, age FROM people WHERE age BETWEEN 13 AND 19")

// The columns of a row in the result can be accessed by field index
teenagersDF.map(teenager => "Name: " + teenager(0)).show()
// +------------+
// |       value|
// +------------+
// |Name: Justin|
// +------------+

// or by field name
teenagersDF.map(teenager => "Name: " + teenager.getAs[String]("name")).show()
// +------------+
// |       value|
// +------------+
// |Name: Justin|
// +------------+

// No pre-defined encoders for Dataset[Map[K,V]], define explicitly
implicit val mapEncoder = org.apache.spark.sql.Encoders.kryo[Map[String, Any]]
// Primitive types and case classes can be also defined as
// implicit val stringIntMapEncoder: Encoder[Map[String, Any]] = ExpressionEncoder()

// row.getValuesMap[T] retrieves multiple columns at once into a Map[String, T]
teenagersDF.map(teenager => teenager.getValuesMap[Any](List("name", "age"))).collect()
// Array(Map("name" -> "Justin", "age" -> 19))
```










