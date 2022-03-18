---
title: Data Scientist | Spark Learning
date: 2020-08-25 22:59:15
tags: [Data Scientist, Spark]
categories: Data Scientist
math: true
mathjax: true
---

<center></center>
<!--more-->





# Spark
Spark包括SQL和处理结构化数据的库（Spark SQL）、机器学习库（MLlib）、流处理库（Spark Streaming和较新的结构化流式处理），以及图分析（GraphX）的库。

此外还有数百种开源外部库：
- 用于各种存储系统的连接器
- 机器学习算法
- ……

[spark-package.org](https://spark-packages.org/)提供了外部库的索引。

Spark支持：
- 批处理应用程序（基于函数式编程的API）
- 交互式数据处理和即席（ad-hoc）查询（将Scala解释器插入Spark，可以提供一个高可用的交互式系统，用于在数百台机器上运行查询）
- Shark系统：可以在Spark上运行SQL查询并满足分析师与数据科学家的交互式应用的引擎

## Spark四大组件
### Spark Streaming
Spark Streaming是Spark平台上针对实时数据进行流式计算的组件，提供了丰富的处理数据流的API

### Spark SQL
Spark SQL是Spark用来操作结构化数据的组件
- 通过Spark SQL，用户可以使用SQL或者Apache Hive版本的SQL语言（HQL）来查询数据
- Spark SQL支持多种数据源类型，例如Hive表、Parquet以及JSON等
- 用户可以在单个的应用中同时进行SQL查询和复杂的数据分析

### GraphX
GraphX是Spark面向图计算提供的框架与算法库。

### MLlib
MLlib是Spark提供的一个机器学习算法库，其中包含了多种经典、常见的机器学习算法，主要有分类、回归、聚类、协同过滤等。

# 运行
- 可以使用Python、Java、Scala、R或SQL等语言来运行Spark
- Spark本身是用Scala编写的，并且运行在Java虚拟机（JVM）上

可以使用2种方法来运行Spark：
1. 在电脑上下载并安装Apache Spark
   1. 安装Java、Python
   2. 在[官网](http://spark.apache.org/downloads.html)选择“Pre-built for Hadoop 3.2 and later”，单击“Download Spark”
   3. 解压下载的文件
2. 在Databricks Community Edition[^1]上运行基于Web的版本
   1. 按照[Spark:The Definitive Guide](https://github.com/databricks/Spark-The-Definitive-Guide)中的操作说明，通过Web界面使用Scala、Python、SQL或R来运行Spark程序，还可以将得到的处理结果可视化

[^1]:一种用于学习Spark的免费云环境

# 交互式控制台
Spark支持不同编程语言的交互式控制台：
- Python
- Scala
- SQL

## Python控制台
- 需要安装Python 2或Python 3
- 在Spark的主目录下运行：
```shell
./bin/pyspark
```
然后输入`spark`并按回车键，将看到打印的`SparkSession`对象。

## Scala控制台
- 在Spark的主目录下运行：
```shell
./bin/spark-shell
```
然后输入`spark`并按回车键，将看到打印的`SparkSession`对象。

## SQL控制台
- 在Spark的主目录下运行：
```shell
./bin/spark-sql
```

# 基本架构
- Spark是一种管理和协调跨多台计算机的计算任务的软件框架

## 应用程序
Spark应用程序由<u>一个驱动器进程</u>和<u>一组执行器进程</u>组成
1. **一个驱动器进程**
   - 驱动器进程运行`main()`函数，位于集群中的一个节点上，负责：
     - 维护Spark应用程序的相关信息
     - 回应用户的程序或输入
     - 分析任务并分发给若干执行器进行处理
   - 驱动器是Spark应用程序的核心，在应用程序执行的整个生命周期中维护着所有相关信息
2. **一组执行器进程**
   - 执行器负责执行驱动器分配的实际计算工作
   - 每个执行器只负责两件事：
     - 执行由驱动器分配给它的代码
     - 并将该执行器的计算状态报告给运行驱动器的节点

page 19的插图

- 用户可以配置指定每个节点上运行多少个执行器
- Spark还可本地运行，此时，驱动器和执行器只是简单的进程，可以位于同一台机器或不同的机器上
- Spark使用一个集群管理器来跟踪可用的资源；集群管理器可以是3个核心集群管理器中的任意一个：
  - 独立集群管理器
  - YARN
  - Mesos

# Spark API
Spark有两套基本的API：
- 低级“非结构化”API
- 更高级别的结构化API

## 多语言支持
Spark API支持多种编程语言：
- Scala：Spark主要用Scala编写，Scala是Spark的默认语言
- Java
- Python：Python几乎支持所有Scala支持的结构
- SQL：Spark支持ANSI SQL 2003标准的一个子集
- R：Spark有两个常用的R库
   1. SparkR：是Spark核心之一
   2. sparklyr：R语言开源社区维护的包

page 21的插图

## SparkSession
可以通过名为SparkSession的驱动器来控制Spark应用程序，需要创建一个SparkSession实例用来在集群中执行用户定义的操作
- 每一个Spark应用程序都需要一个SparkSession与之对应
- 在Scala和Python中，启动控制台时，SparkSession就被实例化为一个名叫`spark`的对象

```scala
// in scala
spark

res0: org.apache.spark.sql.SparkSession = org.apache.spark.sql.SparkSession@...
```

```python
## in python
spark

## 返回
<pyspark.sql.session.SparkSession at 0x7efda4c1ccd0>
```

转换操作和动作操作的逻辑结构是DataFrame和Datset，执行一次转换操作都会创建一个新的DataFrame或Dataset，而动作操作则会触发计算，或将DataFrame和Dataset转换成本地语言类型。

## 转换操作

```scala
// in scala
// 查找当前DataFrame中的所有偶数
val divisBy2 = myRange.where("number % 2 = 0")
```


```python
## in python
## 查找当前DataFrame中的所有偶数
divisBy2 = myRange.where("number % 2 = 0")
```

- 这些转换并没有实际输出，只是抽象转换
- **转换操作**是使用Spark表达业务逻辑的核心
  
有两类转换操作：
1. **窄转换**：指定窄依赖关系（narrow dependency）的转换操作
   - 一对一映射
   - 每个输入分区仅决定一个输出分区的转换
   - Spark将自动执行流水线处理——如果在DataFrame上指定了多个过滤操作，它们将全部在内存中执行
   - 补充24页 插图
2. **宽转换**：指定宽依赖关系（wide dependency）的转换操作
   - 一对多映射
   - 每个输入分区决定了多个输出分区
   - 也被称为**洗牌（shuffle）操作**，会在整个集群中执行互相交换分区数据的功能
   - 当执行shuffle操作时，Spark将结果写入磁盘

### 惰性评估
**惰性评估**（lazy evaluation）：等到绝对需要时才执行计算

在Spark中，当用户表达一些对数据的操作时，不是立即修改数据，而是建立一个作用到原始数据的转换计划。Spark首先会将这个计划编译为可以在集群中高效运行的流水线式的物理执行计划，然后等待，直到最后时刻才开始执行代码。
- 可以优化整个从输入端到输出端的数据流

> 如：DataFrame的谓词下推（predicate pushdown）：假设构建了一个含有多个转换操作的Spark作业，并在最后指定了一个过滤操作，且这个过滤操作只需要数据源中的某一行数据，则最有效的方法就是在最开始仅访问我们需要的单个记录，Spark会通过自动下推这个过滤操作来优化整个物理执行计划

## 动作操作
- 运行一个动作操作（action）以触发计算
- 一个动作指示Spark在一系列转换操作后计算一个结果

最简单的动作操作是：`count()`，计算一个DataFrame中的记录总数
```
divisBy2.count()
```

动作有三类：
- 在控制台中查看数据的动作
- 在某个语言中将数据汇集为原生对象的动作
- 写入输出数据源的动作

## Spark用户接口
可以通过Spark的Web UI监控一个作业的进度
- Spark UI占用驱动器节点的4040端口
- 如果在本地模式下运行，可以通过http://localhost:4040 访问Spark Web UI
- Spark UI上显示了Spark作业的运行状态、执行环境和群集状态等信息

一个Spark作业包含一系列转换操作，并由一个动作操作触发，并可以通过Spark UI监视该作业。

Spark的核心抽象：
- DataFrame
- Dataset
- SQL表
- 弹性分布式数据集（RDD，Resident Distributed Datasets）

这些不同的抽象都表示分布式数据集合，其中最简单、最有效的是DataFrame，它支持所有语言。

# 结构化API
结构化API指以下三种核心分布式集合类型的API：
1. （类型化的）Dataset类型
2. （非类型化的）DataFrame类型
3. SQL表和视图

- 大多数结构化API均适用于批处理和流处理
- 使用结构化API编写代码时，可以从批处理程序转换为流处理程序，反之亦然
- 结构化API是在编写大部分数据处理程序时会用到的基础抽象概念
- Spark类型直接映射到不同语言API，并且针对Scala、Java、Python、SQL和R语言，都有一个对应的API查找表
- 即使通过Python或R语言来使用Spark结构化API，大多数情况下也是操作Spark类型而非Python类型


DataFrame和Dataset是具有行和列类似于（分布式）数据表的集合类型。
- 所有列的函数相同（可以用null来指定缺省值），并且某一列的类型必须在所有行中保持一致
- Spark中的DataFrame和Dataset代表不可变的数据集合，可以通过它指定对特殊位置数据的操作（操作将以惰性评估方式执行）

**行**（Row）：一行对应一个数据记录
- DataFrame中的每条记录都必须是Row类型
- 可以通过SQL手动创建、从弹性分布式数据集（RDD）提取、从数据源手动创建这些行


**列**：表示一个简单类型（eg：整数、字符串），或者一个复杂类型（eg：数组、map映射），或者空值null



## 核心对象
### DataFrame
DataFrame是最常见的结构化API，是包含行和列的数据表
- 说明DataFrame的列和列类型的规则被称为**模式**（schema）
- DataFrame与电子表格不同
  - 电子表格位于一台计算机上，而Spark DataFrame可以跨越数千台计算机
- R DataFrame和Python DataFrame存在于一台机器上，而不是多台机器上
- Spark可以将Python DataFrame或R DataFrame转换为Spark DataFrame
- DataFrame实际是有类型的，只是Spark完全负责维护它们的类型，仅在运行时检查这些类型是否与schema中指定的类型一致
- 在Scala版本的Spark中，DataFrame是一些Row类型的Dataset的集合
  - Row类型：是Spark用于支持内存计算而优化的数据格式




```scala
// in scala
// 创建一组固定范围的数字
// DataFrame
val myRange = spark.range(1000).toDF("number")
```

```python
## in python
## 创建一组固定范围的数字
## DataFrame
myRange = spark.range(1000).toDF("number")
```

### Dataset
- 类型安全的结构化API，用于在Java和Scala中编写静态类型的代码
- 在Dataset上调用`collect`或`take`函数时，将会收集Dataset中合适类型的对象，而不是DataFrame的Row对象
- Dataset仅适用于基于Java虚拟机（JVM）的语言（如Scala和Java），并通过case类或Java beans指定类型
- Python版本和R语言版本的Spark并不支持Dataset，所有东西都是DataFrame


### SQL表和视图
表和视图与DataFrame基本相同，所以通常在DataFrame上执行SQL操作，而不是用DataFrame专用的执行代码

## 执行
结构化API执行过程
61页
待补充

### 逻辑计划

### 物理计划

# Spark类型
## Java类型
```java
// 使用正确的Java类型
import org.apache.spark.sql.types.DataTypes;
ByteType x = DataTypes.ByteType;
```

Java类型参考表：

|数据类型|值类型|获取或创建数据类型的API|
|:-----|:-----|:----|
|ByteType|byte或Byte|`DataTypes.ByteType`|
|ShortType|short或Short|`DataTypes.ShortType`|
|IntegerType|int或Integer|`DataTypes.IntegerType`|
|LongType|long或Long|`DataTypes.LongType`|
|FloatType|float或Float|`DataTypes.FloatType`|
|DoubleType|double或Double|`DataTypes.DoubleType`|
|DecimalType|java.math.BigDecimal|`DataTypes.createDecimalType()`|
|StringType|String|`DataTypes.StringType`|
|BinaryType|byte[]|`DataTypes.BinaryType`|
|BooleanType|boolean或Boolean|`DataTypes.BooleanType`|
|TimestampType|java.sql.TimeStamp|`DataTypes.TimestampType`|
|DateType|java.sql.Date|`DataTypes.DateType`|
|ArrayType|java.util.List|`DataTypes.createArrayType(elementType [, containsNull])`<br>containsNull的默认值为True|
|MapType|java.util.Map|`DataTypes.createMapType(keyType, valueType, [valueContainsNull])`<br>valueContainsNull的默认值为True|
|StructType|org.apache.spark.sql.Row|`DataTypes.createStructType(fields)`<br>`field`是一个包含多个StructFile的Array，并且任意两个StructField不能同名|
|StructField|该字段对应Scala数据类型<br>eg：int是IntegerType的StructField|`DataTypes.createStructField(name, dataType, [nullable])`<br>`nullable`指定该field是否可以为空值，默认值为True|

## Python类型
```python
## 使用正确的Python类型
from pyspark.sql.types import *
b = ByteType()
```

Python类型参考表：

|数据类型|值类型|获取或创建数据类型的API|
|:-----|:-----|:----|
|ByteType|int或long<br>1. 数字在运行时转换为1字节的带符号整数<br>2. 数字范围为-128~127，即$-2^7~2^7-1$|`ByteType()`|
|ShortType|int或long<br>1. 数字在运行时将转换为2字节带符号的整数<br>2. 数字范围为-32768~32767，即$-2^{15}~2^{15}-1$|`ShortType()`|
|IntegerType|int或long<br>若使用IntegerType()，则太大的数字将被Spark SQL拒绝，则最好使用LongType()|`IntegerType()`|
|LongType|long<br>1. 数字在运行时将转换为8字节带符号整数<br>2. 数字范围为$-9223372036854775808~9223372036854775807$，即$-2^{63}~2^{63}-1$|`LongType()`|
|FloatType|float<br>数字在运行时将转换为4字节的单精度浮点数|`FloatType()`|
|DoubleType|float|`DoubleType()`|
|DecimalType|decimal.Decimal|`DecimalType()`|
|StringType|string|`StringType()`|
|BinaryType|bytearray|`BinaryType()`|
|BooleanType|bool|`BooleanType()`|
|TimestampType|datetime.datetime|`TimestampType()`|
|DateType|datetime.date|`DateType()`|
|ArrayType|list, tuple, array|`ArrayType(elementType, [containsNull])`<br>containsNull的默认值为True|
|MapType|字典|`MapType(keyType, valueType, [valueContainsNull])`<br>valueContainsNull的默认值为True|
|StructType|列表或元组|`StructType(fields)`<br>`field`是一个包含多个StructFile的list，并且任意两个StructField不能同名|
|StructField|该字段对应Python数据类型<br>eg：int是IntegerType的StructField|`StructField(name, dataType, [nullable])`<br>`nullable`指定该field是否可以为空值，默认值为True|




## Scala类型
```scala
// 可使用正确的Scala类型
import org.apache.spark.sql.types._val
b = ByteType
```

Scala类型参考表：

|数据类型|值类型|获取或创建数据类型的API|
|:-----|:-----|:----|
|ByteType|Byte|`ByteType`|
|ShortType|Short|`ShortType`|
|IntegerType|Int|`IntegerType`|
|LongType|Long|`LongType`|
|FloatType|Float|`FloatType`|
|DoubleType|Double|`DoubleType`|
|DecimalType|java.math.BigDecimal|`DecimalType`|
|StringType|String|`StringType`|
|BinaryType|Array[Type]|`BinaryType`|
|BooleanType|Boolean|`BooleanType`|
|TimestampType|java.sql.TimeStamp|`TimestampType`|
|DateType|java.sql.Date|`DateType`|
|ArrayType|scala.collection.Seq|`ArrayType(elementType, [containsNull])`<br>containsNull的默认值为True|
|MapType|scala.collection.Map|`MapType(keyType, valueType, [valueContainsNull])`<br>valueContainsNull的默认值为True|
|StructType|org.apache.spark.sql.Row|`StructType(fields)`<br>`field`是一个包含多个StructFile的Array，并且任意两个StructField不能同名|
|StructField|该字段对应Scala数据类型<br>eg：int是IntegerType的StructField|`StructField(name, dataType, [nullable])`<br>`nullable`指定该field是否可以为空值，默认值为True|




# DataFrame
DataFrame由记录（record）组成
- record是Row类型
- 一条record由多列组成
- **分区**定义了DataFrame以及Dataset在集群上的物理分布
- **划分模式**定义了partition的分配方式
- 当使用DataFrame时，向驱动器请求行的命令总是返回一个或多个Row类型的行数据

## 模式
模式（schema）定义了DataFrame列的名称以及列的数据类型，可以由数据源来定义模式（即读时模式，schema-on-read），也可以由我们自己来显式定义。

实际应用场景决定了定义Schema的方式：
- 当应用于即席（Ad-hoc）分析时，使用读时模式
  - 在处理如csv和json等纯文本（无类型）时速度较慢
- 当使用Spark进行生产级别ETL（Extract提取、Transform转换、Load加载）时，使用显示定义

一个模式是由许多字段（StructField）构成的StructType；这些字段是StructField，具有名称、类型、布尔标志（指定该列是否可以包含缺失值或空值），且用户可指定与该列关联的元数据（metadata）。

- 如果在运行时，数据的类型与定义的Schema模式不匹配，Spark将抛出一个错误
- 通过`printSchema()`函数查询DataFrame的模式
- 只有DataFrame具有模式，行对象本身没有模式


```scala
// in scala
// 创建DataFrame并指定模式
import org.apache.spark.sql.types.{StructField, StructType, StringType, LongType}
import org.apache.spark.sql.types.Metadata

val myManualSchema = StructType(Array(
   StructField("DEST_COUNTRY_NAME", StringType, true),
   StructField("ORIGIN_COUNTRY_NAME", StringType, true),
   StructField("count", LongType, false, 
   Metadata.fromJason("{\"hello\":\"world\"}"))
))
val df = spark.read.format("json").schema(myManualSchema)
   .load("/data/flight-data/json/2015-summary.json")
```

```python
# in python
# 创建DataFrame并指定模式
from pyspark.sql.types import StructField, StructType, StringType, LongType

myManualSchema = StructType([
   StructField("DEST_COUNTRY_NAME", StringType(), True),
   StructField("ORIGIN_COUNTRY_NAME", StringType(), True),
   StructField("count", LongType(), False, metadata={"hello":"world"})
])
df = spark.read.format("json").schema(myManualSchema)\
   .load("/data/flight-data/json/2015-summary.json")
```

## 列
Spark中的列与电子表格、R dataframe、pandas DataFrame中的列类似，可以对DataFrame中的列进行选择、转换操作和删除，并将这些操作表示为**表达式**。

对Spark来说，列是逻辑结构，只是表示根据表达式为每个记录计算出的值。

- 可通过`col`函数、`column`函数构造和引用列
- 列和数据表的解析在分析器阶段发生
- Scala还可使用下列表达式创建列
  - 符号`$`将字符串指定为表达式
  - 符号`'`指定一个symbol，是Scala引用标识符的特殊结构
```scala
$"myColumn"
'myColumn
```
- 显示应用列：
  ```python
  # 引用某DataFrame的某一列
   df.col("columnName")
  ```


## 行
- 只有DataFrame具有模式，行（Row）对象本身没有模式
- 手动创建Row对象，必须按照该行所属的DataFrame的列顺序来初始化Row对象

```scala
// in scala
// 创建Row对象
import org.apache.spark.sql.Row
val myRow = Row("Hello", null, 1, false)
```

```python
# in python
# 创建Row对象
from pyspark.sql import Row
myRow = Row("Hello", None, 1, False)
```

- 使用Scala或Java访问行时，需要使用辅助方法或显式地指定值类型
- 使用Python或R访问行时，访问行时，值被自动转化为正确的类型

```scala
// in scala
// 访问Row对象
myRow(0)  // 任意类型
myRow(0).asInstanceOf[String]  // 字符串
myRow.getString(0)  // 字符串
myRow.getInt(2)  // 整型
```

```python
# 访问Row对象
myRow[0]
myRow[2]
```


## 表达式
表达式（expression）是对一个DataFrame中某一个记录的一个或多个值的一组转换操作。

- 最简单的表达式：通过`expr`函数创建的表达式，仅仅是一个DataFrame列的引用。即，`expr("someCol")`等同于`col(someCol)`。
- `expr`函数可以将字符串解析成转换操作和列引用，也可以在之后将其传递到下一步的转换操作
> `expr("someCol - 5")`与`col("someCol") - 5`、`expr("someCol") - 5`，都是相同的转换操作，Spark将它们编译为表示操作顺序的逻辑树

1. 列只是表达式
2. 列与对这些列的转换操作被编译后生成的逻辑计划，与解析后的表达式的逻辑计划是一样的

## 转换操作

图
70页
待补充

- 创建DataFrame：`createDataFrame()`
- 在Scala中，可以利用Spark的隐式方法（使用`implicit`关键字），对Seq类型执行`toDF`函数来实现
  - 但是该方法对`null`类型的支持并不稳定，并不推荐在实际生产中使用
```scala
val myDF = Seq(("Hello", 2, 1L)).toDF("col1", "col2", "col3")
```

- `select`、`selectExpr`支持在DataFrame上执行类似数据表的SQL查询
- `select`：处理列或表达式
- `selectExpr`：处理字符串表达式

# 函数
## apply()
获取指定字段
- 返回对象为Column类型
- 只能获取一个字段
- 而`col`、`column`可以获取多个指定字段


## cast()
用于更改列的类型（来转换数据类型）

```scala
df.withColumn("count", col("count").cast("long"))
```
等价于
```sql
SELECT *, CAST(count AS LONG) AS count2 FROM dfTable;
```

## coalesce()
合并操作
- 不会导致数据的全面洗牌，但是会尝试合并分区

```python
# in scala or Python
df.repartition(5, col("DEST_COUNTRY_NAME")).coalesce(2)
```


## collectAsList()
获取所有数据到List



## columns
使用属性`columns`查询DataFrame的所有列

```scala
// in Scala
spark.read.format("json").load("/data/flight-data/json/2015-summary.json").columns
```


## contains()
检查在某列上是否存在某字符串
- Scala函数
- 返回Boolean值
- 在Python和SQL中，应使用`instr`函数

```scala
// in Scala
val containsBlack = col("Description").contains("BLACK")
val containsWhite = col("DESCRIPTION").contains("WHITE")
df.withColumn("hasSimpleColor", containsBlack.or(containsWhite))
  .where("hasSimpleColor")
  .select("Description").show(3, false)
```

## createDataFrame()
创建DataFrame

```scala
// in Scala
import org.apache.spark.sql.Row
import org.apache.spark.sql.types.{StructField, StructType, StringType, LongType}
val myManualSchema = new StructType(Array(
   new StructField("some", StringType, true),
   new StructField("col", StringType, true),
   new StructField("names", LongType, false)))
val myRows = Seq(Row("Hello", null, 1L))
val myRDD = spark.sparkContext.parallelize(myRows)
val myDf = spark.createDataFrame(myRDD, myManualSchema)
myDf.show()
```

```python
# in Python
from pyspark.sql import Row
from pyspark.sql.types import StructField, StructType, StringType, LongType

myManualSchema = StructType([
   StructField("some", StringType(), True),
   StructField("col", StringType(), True),
   StructField("names", LongType(), False)
])
myRow = Row("Hello", None, 1)
myDf = spark.createDataFrame([myRow], myManualSchema)
myDf.show()
```

## createOrReplaceTempView()
创建临时视图，便于用SQL访问

```scala
// in Scala
val df = spark.read.format("json")
   .load("/data/flight-data/json/2015-summary.json")
df.createOrReplaceTempView("dfTable")
```

```python
# in Python
df = spark.read.format("json").load("/data/flight-data/json/2015-summary.json")
df.createOrReplaceTempView("dfTable")
```

## def
自定义函数

```scala
// in Scala
val udfExampleDF = spark.range(5).toDF("num")
def power3(number:Double):
    Double = number * number * number
power3(2.0)
```

```python
# in Python
udfExampleDF = spark.range(5).toDF("num")
def power3(double_value):
    return double_value ** 3
power3(2.0)
```

目前到115页


## distinct()
去重

```python
# in Python
df.select("ORIGIN_COUNTRY_NAME", "DEST_COUNTRY_NAME").distinct().count()
```

```sql
-- in SQL
SELECT COUNT(DISTINCT(ORIGIN_COUNTRY_NAME, DEST_COUNTRY_NAME)) FROM dfTable;
```


## drop()
删除列；可以通过传入多个列作为参数来同事删除多个列

```python
df.drop("ORIGIN_COUNTRY_NAME")
```

## eqNullSafe()



## euqalTo()
Scala中的“等于”，等价于`===`

```scala
// in Scala
import org.apache.spark.sql.functions.col
df.where(col("InvoiceNo").equalTo(536365))
   .select("InvoiceNo", "Description")
   .show(5, false)
// 等价于
df.where(col("InvoiceNo") === 536365)
   .select("InvoiceNo", "Description")
   .show(5, false)
```

## explain()
- 可以通过`explain`函数观察到Spark是如何执行查询操作的
- 调用某个DataFrame的`explain`操作会显示DataFrame的来源

```scala
# in scala
flightData.sort("count").explain()
==Physical Plan==
*Sort [count#195 ASC NULLS FIRST], true, 0
+- Exchange rangepartitioning(count#195 ASC NULLS FIRST, 200)
   +- FileScan csv [DEST_COUNTRY_NAME#193, ORIGIN_COUNTRY_NAME#194, count#195] ...
```

## first()
在DataFrame上调用`first()`查看一行（获取第一行记录）
```
df.first()
```

## groupBy()

## head()
- `head`：获取第一行记录
- `head(n: Int)`：获取前n行记录

## leq()
小于等于

```scala
// in Scala
import org.apache.spark.sql.functions.{expr, not, col}
df.withColumn("isExpensive", not(col("UnitPrice").leq(250)))
  .filter("isExpensive")
  .select("Description", "UnitPrice").show(5)
df.withColumn("isExpensive", expr("NOT UnitPrice <= 250"))
  .filter("isExpensive")
  .select("Description", "UnitPrice").show(5)

```

## limit()
限制提取的记录数目

```scala
// in Scala or Python
df.limit(5).show()
df.orderBy(expr("count desc")).limit(6).show()
```

```sql
-- in SQL
SELECT * FROM dfTable LIMIT 6;
SELECT * FROM dfTable ORDER BY count desc LIMIT 6;
```


## na
### drop()
删除包含NULL的行
- 参数`any`：当存在一个值是`NULL`时，就删除该行
- 参数`all`：当所有的值为`NULL`或`NaN`时，才删除该行
- 也可指定某几列，对这些列进行删除空值操作

```
df.na.drop()
df.na.drop("any")
df.na.drop("all")
df.na.drop("all", Seq("StockCode", "InvoiceNo"))  // in Scala
df.na.drop("all", subset = ["StockCode", "InvoiceNo"])  # in Python
```

```sql
-- 在SQL中需要逐列删除包含NULL的行
SELECT * FROM dfTable WHERE Description IS NOT NULL;
```

### fill()
可以通过指定一个映射（用一个特定值和一组列），用一组值填充一列或多列
- 可以使用Scala的Map映射实现针对不同的列指定不同的映射方案

```
df.na.fill("All Null values become this string") // 用字符串替换列中的所有NULL值
df.na.fill(5:Integer)  // 用5填充Integer类型的列中的NULL值
df.na.fill(5:Double)  // 用5填充Double类型的列中的NULL值
df.na.fill(5， Seq("StockCode"， "InvoiceNo"))  // 指定多列

val fillColValues = Map("StockCode" -> 5 ， "Description" -> "No Value")  // 指定不同的替换值
df.na.fill(fillColValues)
```

### replace()
根据当前值，替换掉某列中的所有值
- 要求替换值与原始值的类型相同

```scala
// in Scala
df.na.replace("Description"， Map("" -> "UNKNOWN"))
// 将Description列中的空值替换为"UNKNOWN"
```

```python
# in Python
df.na.replace([""]， ["UNKNOWN"]， "Description")
```





## or()
或
- `or`语句需要在同一个语句中指定

```scala
// in Scala
val priceFilter = col("UnitPrice") > 600  // 筛选条件1
val descripFilter = col("Description").contains("POSTAGE")  // 筛选条件2
df.where(col("StockCode").isin("DOT")).where(priceFilter.or(descripFilter))
   .show()
```

```python
# in Python
from pyspark.sql.functions import instr
priceFilter = col("UnitPrice") > 600
descripFilter = instr(df.Description, "POSTAGE") >= 1
df.where(df.StockCode.isin("DOT")).where(priceFilter | descripFilter).show()  ## Python的“或”为“|”
```

```sql
-- in SQL
SELECT * FROM dfTable WHERE StockCode in ("DOT") AND(UnitPrice > 600 OR instr(Description, "POSTAGE") >= 1)
```

## orderBy()
对DataFrame的值进行排序
- `sort`和`orderBy`是等价的，均接收列表达式、字符串，以及多个列
- 默认按升序（asc）排序
- 如果要指定降序排序，则需使用`desc()`函数

```scala
// in Scala
df.sort("count").show(5)
df.orderBy("count", "DEST_COUNTRY_NAME").show(5)
df.orderBy(col("count"), col("DEST_COUNTRY_NAME")).show(5)

import org.apache.spark.sql.functions.{desc, asc}
df.orderBy(expr("count desc")).show(2)  // 指定降序排序
df.orderBy(desc("count"), asc("DEST_COUNTRY_NAME")).show(2)  // 分别指定降序、升序排序
```

```python
# in Python
df.sort("count").show(5)
df.orderBy("count", "DEST_COUNTRY_NAME").show(5)
df.orderBy(col("count"), col("DEST_COUNTRY_NAME")).show(5)

# in Python
from pyspark.sql.functions import desc, asc
df.orderBy(expr("count desc")).show(2)
df.orderBy(col("count").desc(), col("DEST_COUNTRY_NAME").asc()).show(2)
```

```sql
-- in SQL
SELECT * FROM dfTable ORDER BY count DESC, DEST_COUNTRY_NAME ASC LIMIT 2;
```

- 可以指定空值在排序列表中的位置
  - `asc_nulls_first`指示空值排在升序排列之前
  - `desc_nulls_first`指示空值排在降序排列之前
  - `asc_nulls_last`指示空值排在升序排列后面


## printSchema()
查询DataFrame的模式（schema）
```scala
// in scala
val df = spark.read.format("json")
   .load("/data/flight-data/json/2015-summary.json")
// 查询DataFrame的模式
df.printSchema()
```

## randomSplit()
随机分割
- 需要设置分割比例
- 如果分割比例的和不为1，则比例参数会被归一化

```scala
// 将DataFrame按25%和75%的比例分割
val dataFrames = df.randomSplit(Array(0.25, 0.75), 8)  // 其中8为seed参数
```

```python
# in Python
dataFrames = df.randomSplit([0.25, 0.75], 8)
```


## range()

## read
读取数据
- 是一种转换操作
- 窄依赖
- 也是一种惰性操作——Spark并没有马上读取数据，直到在DataFrame上调用动作操作后才会真正读取数据

`option`参数：
- `inferSchema=true`：模式推理，让Spark猜测DataFrame的模式
- `header=true`：指定文件的第一行为文件头

```scala
// in scala
val flightData = spark
   .read
   .option("inferSchema", "true")
   .option("header", "true")
   .csv("/data/flight-data/csv/summary.csv")
```

```python
# in python
flightData = spark\
   .read\
   .option("inferSchema", "true")\
   .option("header", "true")\
   .csv("/data/flight-data/csv/summary.csv")
# 注意：python换行的话，行末需要加转义
```

## repartition()
重新分区

```scala
// in Scala or Python
df.repartition(5)  // 设置分区数为5
df.repartition(col("DEST_COUNTRY_NAME"))  // 按照某列DEST_COUNTRY_NAME进行分区
df.repartition(5, col("DEST_COUNTRY_NAME"))  // 指定分区数和列
```

## sample()
随机抽样；按一定比例从DataFrame中随机抽取一部分行
- 参数`withReplacement`指定是否放回抽样
   - `true`为有放回抽样/有重复抽样
   - `false`为无放回抽样/无重复抽样 

```scala
// in scala
val seed = 5
val withReplacement = false // 无重复抽样
val fraction = 0.5 // 抽取50%
df.sample(withReplacement, fraction, seed).count()
```

```python
# in Python
seed = 5
withReplacement = False
fraction = 0.5
df.sample(withReplacement, fraction, seed).count()
```



## select()
处理列或表达式
- 将待处理的列名字符串作为参数传递
- 添加多个列名字符串，可以选择多个列
- 不能混淆使用Column对象和字符串

```scala
// in Scala
df.select("DEST_COUNTRY_NAME").show(2)
// 选择多个列
df.select("DEST_COUNTRY_NAME", "ORIGIN_COUNTRY_NAME").show(2)
// 可以通过多种不同的方式引用列
import org.apache.spark.sql.functions.{expr, col, column}
df.select(
   df.col("DEST_COUNTRY_NAME"),
      col("DEST_COUNTRY_NAME"),
      column("DEST_COUNTRY_NAME"),
      'DEST_COUNTRY_NAME,
      $"DEST_COUNTRY_NAME",
      expr("DEST_COUNTRY_NAME"))
   .show(2)
```

```python
# in Python
df.select("DEST_COUNTRY_NAME").show(2)
# 选择多个列
df.select("DEST_COUNTRY_NAME", "ORIGIN_COUNTRY_NAME").show(2)
# 可以通过多种不同的方式引用列
from pyspark.sql.functions import expr, col, column
df.select(
   expr("DEST_COUNTRY_NAME"),
   col("DEST_COUNTRY_NAME"),
   column("DEST_COUNTRY_NAME"))\
.show(2)
```

```sql
-- in SQL
SELECT DEST_COUNTRY_NAME FROM dfTable LIMIT 2
-- 选择多个列
SELECT DEST_COUNTRY_NAME, ORIGIN_COUNTRY_NAME FROM dfTable LIMIT 2
```

## selectExpr()
可以用于构造复杂表达式来创建DataFrame
- 是常用的接口之一
- 可以添加任何不包含聚合操作的有效SQL语句
- 可以使用系统预定义的聚合函数来指定在整个DataFrame上的聚合操作

```scala
// in scala
df.selectExpr(
   "*", // 包含所有原始表中的列
   "(DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME) as withinCountry"  // 增加新列withinCountry
   )
.show(2)
// 使用系统预定义的聚合函数来指定在整个DataFrame上的聚合操作
df.selectExpr("avg(count)", "count(distinct(DEST_COUNTRY_NAME))").show(2)
```

```python
# in Python
df.selectExpr(
   "*", # 包含所有原始表中的列
   "(DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME) as withinCountry")\
.show(2)
# 使用系统预定义的聚合函数来指定在整个DataFrame上的聚合操作
df.selectExpr("avg(count)", "count(distinct(DEST_COUNTRY_NAME))").show(2)
```

```sql
-- in SQL
SELECT *, (DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME) as withinCountry
FROM dfTable
LIMIT 2;
-- 使用系统预定义的聚合函数来指定在整个DataFrame上的聚合操作
SELECT AVG(count), COUNT(distinct(DEST_COUNTRY_NAME)) FROM dfTable LIMIT 2;
```

## show()
- `show`：默认显示DataFrame的前20条记录
- `show(numRows: Int)`：显示numRows条记录
- `show(truncate: Boolean)`：是否每列最多只显示20个字符，默认为true
   ```python
   df.show(true)  # 空格也算一个字符
   df.show(false)
   ```
- `show(numRows: Int, truncate: Boolean)`：显示numRows条记录，且每列是否最多显示20个字符

## sort()
- 宽依赖(因为行之间需要相互比较和交换)
- 不会修改DataFrame，通过转换DataFrame来返回新的DataFrame

## sql()
使用`spark.sql()`函数在SQL中查询数据，返回新的DataFrame

```scala
// in scala
val sqlWay = spark.sql("""
   SELECT DEST_COUNTRY_NAME, count(1)
   FROM flight_data
   GROUP BY DEST_COUNTRY_NAME
""")

val DataFrameWay = flightData
   .groupby('DEST_COUNTRY_NAME')
   .count()

sqlWay.explain()
== Physical Plan ==
*HashAggregate(keys=[DEST_COUNTRY_NAME#182], functions=[count(1)])
+- Exchange hashpartitioning(DEST_COUNTRY_NAME#182, 5)
   +- *HashAggregate(keys=[DEST_COUNTRY_NAME#182], functions=[partial_count(1)])
      +- *FileScan csv [DEST_COUNTRY_NAME#182] ...

DataFrameWay.explain()
== Physical Plan ==
*HashAggregate(keys=[DEST_COUNTRY_NAME#182], functions=[count(1)])
+- Exchange hashpartitioning(DEST_COUNTRY_NAME#182, 5)
   +- *HashAggregate(keys=[DEST_COUNTRY_NAME#182], functions=[partial_count(1)])
      +- *FileScan csv [DEST_COUNTRY_NAME#182] ...
```

```scala
// in scala
val maxSql = spark.sql("""
   SELECT DEST_COUNTRY_NAME, sum(count) AS destination_total
   FROM flight_data
   GOURP BY DEST_COUNTRY_NAME
   ORDER BY sum(count) DESC
   LIMIT 5
""")
maxSql.show()
```

## take()
`take(n: Int)`：获取前n行数据
- `take`和`takeAsList`会将获得的数据返回到Driver端，使用时应注意数据量，以免Driver发生`OutOfMemoryError`

## takeAsList()
获取前n行数据，并以List形式展现
- `take`和`takeAsList`会将获得的数据返回到Driver端，使用时应注意数据量，以免Driver发生`OutOfMemoryError`


## toDF()


## toLocalIterator()
该函数是一个迭代器，将每个分区的数据返回给驱动器
- 允许以串行的方式一个一个分区地迭代整个数据集
- 使用该函数，且分区很大时，很容易使驱动器节点崩溃并丢失应用程序的状态，代价很大

```python
# in Python
collectDF = df.limit(10)
collectDF.toLocalIterator()
```

## union()
连接/拼接两个DataFrame
- 被连接的两个DataFrame需要具有完全相同的模式和列数

```scala
// in Scala
import org.apache.spark.sql.Row
val schema = df.schema
val newRows = Seq(
   Row("New Country", "Other Country", 5L),
   Row("New Country 2", "Other Country 3", 1L)
)
val parallelizedRows = spark.sparkContext.parallelize(newRows)
val newDF = spark.createDataFrame(parallelizedRows, schema)
df.union(newDF)
   .where("count = 1")
   .where($"ORIGIN_COUNTRY_NAME" =!= "United States")
   .show()
```



## where()
过滤行，与`filter()`等价
- Spark同时执行所有过滤操作（不论过滤条件的先后顺序）

```scala
df.filter(col("count") < 2).show(2)
// 在Scala和Python中等价于
df.where("count < 2").show(2)
```
等价于
```sql
SELECT * FROM dfTable WHERE count < 2 LIMIT 2;
```

```scala
// in scala
df.where(col("count") < 2).where(col("ORIGIN_COUNTRY_NAME") =!= "Croatia")
   .show(2)
```


```python
# in python
df.where(col("count") < 2).where(col("ORIGIN_COUNTRY_NAME") != "Croatia")\
  .show(2)
```

```sql
-- in SQL
SELECT * FROM dfTable WHERE count < 2 AND ORIGIN_COUNTRY_NAME != "Croatia"
LIMIT 2
```

- 将过滤器表示为SQL语句比使用编程式的DataFrame接口更简单



## withColumn()
添加新列
- 包含两个参数：
  1. 列名
  2. 为给定行赋值的表达式
- 还可用于对列重命名

```scala
// in scala or python
df.withColumn("numberOne", lit(1)).show(2) // 添加一个仅包含数字1的列numberOne
// 从已有的列DEST_COUNTRY_NAME新建列Destination并删除旧列DEST_COUNTRY_NAME
df.withColumn("Destination", expr("DEST_COUNTRY_NAME"))
  .drop("DEST_COUNTRY_NAME")
```

```sql
-- in SQL
SELECT *, 1 AS numberOne FROM dfTable LIMIT 2;
```


## withColumnRenamed()
重命名列
- 第一个参数是原始列名称
- 第二个参数是新列名称

```scala
// in Scala
// 将DEST_COUNTRY_NAME重命名为dest
df.withColumnRenamed("DEST_COUNTRY_NAME", "dest")
```












# functions
## alias()
为选择的列起别名
```python
# in Python
from pyspark.sql.functions import expr, pow
fabricatedQuantity = pow(col("Quantity") * col("UnitPrice"), 2) + 5
df.select(expr("CustomerId"), fabricatedQuantity.alias("realQuantity")).show(2)
```

## array_contains()
查询数组是否包含某个值
- 返回`true`或`false`

```scala
// in Scala
import org.apache.spark.sql.functions.array_contains
df.select(array_contains(split(col("Description"), " "), "WHITE")).show(2)
// 将Description按照空格拆分成数组，判断拆分后的数组是否包含字符串"WHITE"
```


```python
# in Python
from pyspark.sql.functions import array_contains
df.select(array_contains(split(col("Description"), " "), "WHITE")).show(2)
```

```sql
SELECT ARRAY_CONTAINS(SPLIT(COL("Desription"), " "), "WHITE") FROM dfTable;
```




## coalesce()
从一组列中选择第一个非空值（第一个非NULL值）

```scala
// in Scala
import org.apache.spark.sql.functions.coalesce
df.select(coalesce(col("Description")， col("CustomerId"))).show()
```

```python
# in Python
from pyspark.sql.functions import coalesce
df.select(coalesce(col("Description")， col("CustomerId"))).show()
```



## col()、column()
构造和引用列（获取指定字段）；需要传入列名
- 返回对象为Column类型

```scala
// in scala
import org.apache.spark.sql.functions.{col, column}
col("someColumnName")
column("someColumnName")
// Scala还可使用下列方式创建列
$"myColumn"
'myColumn
```

```python
# in python
from pyspark.sql.functions import col, column
col("someColumnName")
column("someColumnName")
```

## collect()
从整个DataFrame中获取所有数据

```python
# in Python
collectDF = df.limit(10)
collectDF.take(5) # 获取整数行
collectDF.show() # 更友好的打印
collectDF.show(5, False)
collectDF.collect() # 获取所有的数据
```

## corr()
计算两列的相关系数

```scala
// in Scala
import org.apache.spark.sql.functions.{corr}
df.stat.corr("Quantity", "UnitPrice")
df.select(corr("Quantity", "UnitPrice")).show()
```

```python
# in Python
from pyspark.sql.functions import corr
df.stat.corr("Quantity", "UnitPrice")
df.select(corr("Quantity", "UnitPrice")).show()
```

```sql
-- in SQL
SELECT corr(Quantity, UnitPrice) FROM dfTable;
```

## count
统计记录条数

```
// in Scala
import org.apache.spark.sql.functions.{count}

# in Python
from pyspark.sql.functions import count
```


## current_date()
获取当前日期

```scala
// in Scala
import org.apache.spark.sql.functions.{current_date, current_timestamp}
val dateDF = spark.range(10)
  .withColumn("today", current_date())
  .withColumn("now", current_timestamp())
dateDF.createOrReplaceTempView("dateTable")
```

```python
# in Python
from pyspark.sql.functions import current_date, current_timestamp
dateDF = spark.range(10)\
  .withColumn("today", current_date())\
  .withColumn("now", current_timestamp())
dateDF.createOrReplaceTempView("dateTable")
```

## current_timestamp()
获取当前时间戳

```scala
// in Scala
import org.apache.spark.sql.functions.{current_date, current_timestamp}
val dateDF = spark.range(10)
  .withColumn("today", current_date())
  .withColumn("now", current_timestamp())
dateDF.createOrReplaceTempView("dateTable")
```

```python
# in Python
from pyspark.sql.functions import current_date, current_timestamp
dateDF = spark.range(10)\
  .withColumn("today", current_date())\
  .withColumn("now", current_timestamp())
dateDF.createOrReplaceTempView("dateTable")
```

## date_add()
添加天数

```scala
// in Scala
import org.apache.spark.sql.functions.{date_add, date_sub}
dateDF.select(date_sub(col("today"), 5), date_add(col("today"), 5)).show(1)
```

```python
# in Python
from pyspark.sql.functions import date_add, date_sub
dateDF.select(date_sub(col("today"), 5), date_add(col("today"), 5)).show(1)
```

```sql
-- in SQL
SELECT DATE_SUB(today, 5), DATE_ADD(today, 5) FROM dateTable;
```

## datediff()
查看两个日期之间的间隔时间（返回两个日期之间的天数）
```scala
// in Scala
import org.apache.spark.sql.functions.{datediff, months_between, to_date}
dateDF.withColumn("week_ago", date_sub(col("today"), 7))
  .select(datediff(col("week_ago"), col("today"))).show(1)
dateDF.select(
  to_date(lit("2016-01-01")).alias("start"),
  to_date(lit("2017-05-22")).alias("end"))
  .select(months_between(col("start"), col("end"))).show(1)
```

```python
# in Python
from pyspark.sql.functions import datediff, months_between, to_date
dateDF.withColumn("week_ago", date_sub(col("today"), 7))\
  .select(datediff(col("week_ago"), col("today"))).show(1)
```

```sql
-- in SQL
SELECT 
   to_date('2016-01-01'), 
   months_between('2016-01-01', '2017-01-01'), datediff('2016-01-01', '2017-01-01')
FROM dateTable;
```


## date_sub()
减去天数

```scala
// in Scala
import org.apache.spark.sql.functions.{date_add, date_sub}
dateDF.select(date_sub(col("today"), 5), date_add(col("today"), 5)).show(1)
```

```python
# in Python
from pyspark.sql.functions import date_add, date_sub
dateDF.select(date_sub(col("today"), 5), date_add(col("today"), 5)).show(1)
```

```sql
-- in SQL
SELECT DATE_SUB(today, 5), DATE_ADD(today, 5) FROM dateTable;
```


## desc()
- 降序排列
- 结合`sort`、`orderBy`使用
- `desc`函数返回的是一个Column，而不是一个字符串

```scala
// in scala
import org.apache.spark.sql.functions.desc

flightData
   .groupBy("DEST_COUNTRY_NAME")
   .sum("count")
   .withColumnRenamed("sum(count)", "destination_total")
   .sort(desc("destination_total"))
   .limit(5)
   .show()
```

```python
# in python
from pyspark.sql.functions import desc

flightData\
   .groupBy("DEST_COUNTRY_NAME")\
   .sum("count")\
   .withColumnRenamed("sum(count)", "destination_total")\
   .sort(desc("destination_total"))\
   .limit(5)\
   .show()
```


33页插图


## describe()
返回数值类型字段的描述性统计值（汇总统计信息）
- 返回DataFrame对象
- 返回以下统计值：
  - `count`：样本数
  - `mean`：均值
  - `stddev`：标准差
  - `min`：最小值
  - `max`：最大值
- 如果某列是字符类型，则`mean`和`stddev`为`null`

```python
# in Python or Scala
df.describe().show()
```

## explode()
为输入的数组中的每个值创建一行。如，对`["Hello", "World"], "other col"`实施`explode`后得到
```
"Hello", "other col"
"World", "other col"
```

```scala
// in Scala
import org.apache.spark.sql.functions.{split, explode}
df.withColumn("splitted", split(col("Description"), " "))
  .withColumn("exploded", explode(col("splitted")))
  .select("Description", "InvoiceNo", "exploded").show(2)
```

```python
# in Python
from pyspark.sql.functions import split, explode
df.withColumn("splitted", split(col("Description"), " "))\
  .withColumn("exploded", explode(col("splitted")))\
  .select("Description", "InvoiceNo", "exploded").show(2)
```

```sql
SELECT 
   Description, InvoiceNo, exploded
FROM (
   SELECT *, SPLIT(Description, " ") AS splitted FROM dfTable
)
LATERAL VIEW EXPLODE(splitted) AS exploded;
```

- 可使用`explode()`展开map类型，将其转换为列

```scala
// in Scala
df.select(map(col("Description"), col("InvoiceNo")).alias("complex_map"))
  .selectExpr("explode(complex_map)").show(2)
```

```python
# in Python
df.select(map(col("Description"), col("InvoiceNo")).alias("complex_map"))\
  .selectExpr("explode(complex_map)").show(2)
```
展开结果如下：
```
+--------------------+------+
|                 key| value|
+--------------------+------+
|WHITE HANGING HEA...|536365|
| WHITE METAL LANTERN|536365|
+--------------------+------+
```


## from_json()
解析JSON数据

```scala
// in Scala
import org.apache.spark.sql.functions.from_json
import org.apache.spark.sql.types._  // 加载所有的types

val parseSchema = new StructType(Array(
   new StructField("InvoiceNo", StringType, true),
   new StructField("Description", StringType, true)))
df.selectExpr("(InvoiceNo, Description) as myStruct")
  .select(to_json(col("myStruct")).alias("newJSON"))
  .select(from_json(col("newJSON"), parseSchema), col("newJSON")).show(2)
```

```python
# in Python
from pyspark.sql.functions import from_json
from pyspark.sql.types import *

parseSchema = StructType((
   StructField("InvoiceNo",StringType(),True),
   StructField("Description",StringType(),True)))
df.selectExpr("(InvoiceNo, Description) as myStruct")\
  .select(to_json(col("myStruct")).alias("newJSON"))\
  .select(from_json(col("newJSON"), parseSchema), col("newJSON")).show(2)
```



## get_json_object()
查询JSON对象

```scala
// in Scala
// 创建JSON类型的列
val jsonDF = spark.range(1).selectExpr("""
   '{
      "myJSONKey" : 
      {
         "myJSONValue" : [1, 2, 3]
      }
   }' 
   as jsonString""")


import org.apache.spark.sql.functions.{get_json_object, json_tuple}
jsonDF.select(
   get_json_object(col("jsonString"), "$.myJSONKey.myJSONValue[1]") as "column", // 返回2
   json_tuple(col("jsonString"), "myJSONKey")).show(2)  // 返回{"muJSONValue": [1, 2, 3]}
```

```python
# in Python
# 创建JSON类型的列
jsonDF = spark.range(1).selectExpr("""
'{"myJSONKey" : {"myJSONValue" : [1, 2, 3]}}' as jsonString""")

from pyspark.sql.functions import get_json_object, json_tuple
jsonDF.select(
  get_json_object(col("jsonString"), "$.myJSONKey.myJSONValue[1]") as "column",
  json_tuple(col("jsonString"), "myJSONKey")).show(2)
```








## initcap()
将给定字符串中空格分隔的每个单词首字母大写

```scala
// in Scala
import org.apache.spark.sql.functions.{initcap}
df.select(initcap(col("Description"))).show(2, false)
```

```python
# in Python
from pyspark.sql.functions import initcap
df.select(initcap(col("Description"))).show()
```

```sql
-- in SQL
SELECT initcap(Description) FROM dfTable;
```


## instr()
检查在某列上是否存在某字符串
- 在Scala中使用`contains()`函数

```python
# in Python
from pyspark.sql.functions import instr
containsBlack = instr(col("Description"), "BLACK") >= 1
containsWhite = instr(col("Description"), "WHITE") >= 1
df.withColumn("hasSimpleColor", containsBlack | containsWhite)\
  .where("hasSimpleColor")\
  .select("Description").show(3, False)
```

```sql
-- in SQL
SELECT Description FROM dfTable
WHERE instr(Description, 'BLACK') >= 1 OR instr(Description, 'WHITE') >= 1;
```

## json_tuple()
如果JSON对象只有一层嵌套，则可使用该函数进行查询

```scala
// in Scala
// 创建JSON类型的列
val jsonDF = spark.range(1).selectExpr("""
   '{
      "myJSONKey" : 
      {
         "myJSONValue" : [1, 2, 3]
      }
   }' 
   as jsonString""")


import org.apache.spark.sql.functions.{get_json_object, json_tuple}
jsonDF.select(
   get_json_object(col("jsonString"), "$.myJSONKey.myJSONValue[1]") as "column", // 返回2
   json_tuple(col("jsonString"), "myJSONKey")).show(2)  // 返回{"muJSONValue": [1, 2, 3]}
```

```python
# in Python
# 创建JSON类型的列
jsonDF = spark.range(1).selectExpr("""
'{"myJSONKey" : {"myJSONValue" : [1, 2, 3]}}' as jsonString""")

from pyspark.sql.functions import get_json_object, json_tuple
jsonDF.select(
  get_json_object(col("jsonString"), "$.myJSONKey.myJSONValue[1]") as "column",
  json_tuple(col("jsonString"), "myJSONKey")).show(2)
```




## lit()
创造字面量（literal）（常量值）；把其他语言的类型转换为与其相对应的Spark表示

```scala
// in scala
import org.apache.spark.sql.functions.lit
df.select(expr("*"), lit(1).as("One")).show(2)

df.select(lit(5)， lit("five")， lit(5.0))
```


```python
# in python
from pyspark.sql.functions import lit
df.select(expr("*"), lit(1).alias("One")).show(2)
```

```sql
-- in SQL
SELECT *, 1 AS One FROM dfTable LIMIT 2;
```

## locate()
返回整数位置（从1开始）

```python
from pyspark.sql.functions import expr, locate
simpleColors = ["black", "white", "red", "green", "blue"]
def color_locator(column, color_string):
   return locate(color_string.upper(), column)\
            .cast("boolean")\
            .alias("is_" + color_string)
selectedColumns = [color_locator(df.Description, c) for c in simpleColors]
selectedColumns.append(expr("*"))  # expr()转为Column格式
df.select(*selectedColumns).where(expr("is_white OR is_red"))\
  .select("Description").show(3, False)
```

## lower()
将字符串转为小写

```scala
// in Scala
import org.apache.spark.sql.functions.{lower, upper}
df.select(col("Description"),
   lower(col("Description")),
   upper(lower(col("Description")))).show(2)
```

```python
# in Python
from pyspark.sql.functions import lower, upper
df.select(col("Description"),
   lower(col("Description")),
   upper(lower(col("Description")))).show(2)
```

```sql
-- in SQL
SELECT Description, lower(Description), Upper(lower(Description)) FROM dfTable;
```

## lpad()
在字符串左边添加空格
- 如果`lpad`或`rpad`方法输入的数值参数小于字符串长度，将从字符串的右侧删除字符

```scala
// in Scala
import org.apache.spark.sql.functions.{lit, rpad, lpad}
df.select(
   lpad(lit("HELLO"), 3, " ").as("lp"),
   rpad(lit("HELLO"), 10, " ").as("rp")).show(2)
```

```python
# in Python
from pyspark.sql.functions import lit, rpad, lpad
df.select(
   lpad(lit("HELLO"), 3, " ").alias("lp"),
   rpad(lit("HELLO"), 10, " ").alias("rp")).show(2)
```

```sql
SELECT
   lpad('HELLOOOO ', 3, ' '),
   rpad('HELLOOOO ', 10, ' ')
FROM dfTable;
```


## map()
构建两列内容的键值对映射形式

```scala
// in Scala
import org.apache.spark.sql.functions.map
df.select(map(col("Description"), col("InvoiceNo")).alias("complex_map")).show(2)
```

```python
# in Python
from pyspark.sql.functions import create_map
df.select(create_map(col("Description"), col("InvoiceNo")).alias("complex_map"))\
  .show(2)
```

```sql
SELECT MAP(Description, InvoiceNo) AS complex_map
FROM dfTable
WHERE Description IS NOT NULL;
```

- 可以使用正确的键（key）对键值对进行查询
- 若键（key）不存在则返回NULL

```scala
// in Scala
df.select(map(col("Description"), col("InvoiceNo")).alias("complex_map"))
  .selectExpr("complex_map['WHITE METAL LANTERN']").show(2)  // 查询键'WHITE METAL LANTERN'对应的值
```

```python
# in Python
df.select(map(col("Description"), col("InvoiceNo")).alias("complex_map"))\
  .selectExpr("complex_map['WHITE METAL LANTERN']").show(2)
```

- 可使用`explode()`展开map类型，将其转换为列

```scala
// in Scala
df.select(map(col("Description"), col("InvoiceNo")).alias("complex_map"))
  .selectExpr("explode(complex_map)").show(2)
```

```python
# in Python
df.select(map(col("Description"), col("InvoiceNo")).alias("complex_map"))\
  .selectExpr("explode(complex_map)").show(2)
```
展开结果如下：
```
+--------------------+------+
|                 key| value|
+--------------------+------+
|WHITE HANGING HEA...|536365|
| WHITE METAL LANTERN|536365|
+--------------------+------+
```



## max()
最大值

```scala
// in scala
import org.apache.spark.sql.functions.max

flightData.select(max("count")).take(1)
// 按照count列排序的最大值
```


```python
# in python
from pyspark.sql.functions import max

flightData.select(max("count")).count(1)
```

## mean()
计算均值
```
// in Scala
import org.apache.spark.sql.functions.{mean}

# in Python
from pyspark.sql.functions import mean
```

## min
最小值

```
// in Scala
import org.apache.spark.sql.functions.{min}

# in Python
from pyspark.sql.functions import min
```

## monotonically_increasing_id
位每行添加一个唯一的id
- 从0开始，为每行生成一个唯一值

```scala
// in Scala
import org.apache.spark.sql.functions.monotonically_increasing_id
df.select(monotonically_increasing_id()).show(2)
```

```python
# in Python
from pyspark.sql.functions import monotonically_increasing_id
df.select(monotonically_increasing_id()).show(2)
```

## months_between()
返回两个日期之间相隔的月数

```scala
// in Scala
import org.apache.spark.sql.functions.{months_between, to_date}

dateDF.select(
  to_date(lit("2016-01-01")).alias("start"),
  to_date(lit("2017-05-22")).alias("end"))
  .select(months_between(col("start"), col("end"))).show(1)
```

```python
# in Python
from pyspark.sql.functions import months_between, to_date

dateDF.select(
  to_date(lit("2016-01-01")).alias("start"),
  to_date(lit("2017-05-22")).alias("end"))\
  .select(months_between(col("start"), col("end"))).show(1)
```

```sql
-- in SQL
SELECT 
   to_date('2016-01-01'), 
   months_between('2016-01-01', '2017-01-01'), datediff('2016-01-01', '2017-01-01')
FROM dateTable;
```


## pow()
进行幂运算
`pow(n, k)`：计算$n^k$

```python
# in Python
from pyspark.sql.functions import expr, pow
fabricatedQuantity = pow(col("Quantity") * col("UnitPrice")， 2) + 5
df.select(expr("CustomerId")， fabricatedQuantity.alias("realQuantity")).show(2)
# 或
df.selectExpr(
   "CustomerId",
   "(POWER((Quantity * UnitPrice), 2.0) + 5) as realQuantity").show(2)
```

```sql
-- in SQL
SELECT customerId, (POWER((Quantity * UnitPrice), 2.0) + 5) as realQuantity
FROM dfTable;
```

## regex_extract()
提取符合条件的字符串

```scala
// in Scala
// 提取Description列中第一个被提到的颜色
import org.apache.spark.sql.functions.regexp_extract
val simpleColors = Seq("black", "white", "red", "green", "blue")
val regexString = simpleColors.map(_.toUpperCase).mkString("(", "|", ")")
// "|"在正则表达式中是"或"的意思
df.select(
   regexp_extract(col("Description"), regexString, 1).alias("color_clean"),
   col("Description")).show(2)
```

```python
# in Python
from pyspark.sql.functions import regexp_extract
extract_str = "(BLACK|WHITE|RED|GREEN|BLUE)"
df.select(
   regexp_extract(col("Description"), extract_str, 1).alias("color_clean"),
   col("Description")).show(2)
```

```sql
-- in SQL
SELECT regexp_extract(Description, '(BLACK|WHITE|RED|GREEN|BLUE)', 1), Description
FROM dfTable;
```

## regex_replace()
替换符合条件的字符串

```scala
// in Scala
// 将Description列中的颜色字符串BLACK|WHITE|RED|GREEN|BLUE替换为“COLOR”
import org.apache.spark.sql.functions.regexp_replace
val simpleColors = Seq("black", "white", "red", "green", "blue")
val regexString = simpleColors.map(_.toUpperCase).mkString("|")
// "|"在正则表达式中是"或"的意思
df.select(
   regexp_replace(col("Description"), regexString, "COLOR").alias("color_clean"),
   col("Description")).show(2)
```

```python
# in Python
from pyspark.sql.functions import regexp_replace
regex_string = "BLACK|WHITE|RED|GREEN|BLUE"
df.select(
   regexp_replace(col("Description"), regex_string, "COLOR").alias("color_clean"),
   col("Description")).show(2)
```

```sql
-- in SQL
SELECT
regexp_replace(Description, 'BLACK|WHITE|RED|GREEN|BLUE', 'COLOR') AS color_clean, Description
FROM dfTable;
```

## round()
- 四舍五入
- 默认情况下，如果恰好位于两个数字之间，`round`函数会向上取整
- `bround`函数可以向下取整

```scala
// in Scala
import org.apache.spark.sql.functions.{round， bround}
df.select(round(col("UnitPrice")， 1).alias("rounded")， col("UnitPrice")).show(5)
// 保留1位小数
import org.apache.spark.sql.functions.lit
df.select(round(lit("2.5")), bround(lit("2.5"))).show(2)
// 输出3.0和2.0
```

```python
# in Python
from pyspark.sql.functions import lit, round, bround
df.select(round(lit("2.5")), bround(lit("2.5"))).show(2)
# 输出3.0和2.0
```

```sql
-- in SQL
SELECT round(2.5), bround(2.5);
-- 输出3.0和2.0
```


## rpad()
在字符串右边添加空格
- 如果`lpad`或`rpad`方法输入的数值参数小于字符串长度，将从字符串的右侧删除字符

```scala
// in Scala
import org.apache.spark.sql.functions.{lit, rpad, lpad}
df.select(
   lpad(lit("HELLO"), 3, " ").as("lp"),
   rpad(lit("HELLO"), 10, " ").as("rp")).show(2)
```

```python
# in Python
from pyspark.sql.functions import lit, rpad, lpad
df.select(
   lpad(lit("HELLO"), 3, " ").alias("lp"),
   rpad(lit("HELLO"), 10, " ").alias("rp")).show(2)
```

```sql
SELECT
   lpad('HELLOOOO ', 3, ' '),
   rpad('HELLOOOO ', 10, ' ')
FROM dfTable;
```

## size()
查询数组的大小（长度）

```scala
// in Scala
import org.apache.spark.sql.functions.size
df.select(size(split(col("Description")， " "))).show(2)
```

```python
# in Python
from pyspark.sql.functions import size
df.select(size(split(col("Description")， " "))).show(2)
```


## split()
按照指定的分隔符将字符串分割成数组

```scala
// in Scala
import org.apache.spark.sql.functions.split
df.select(split(col("Description"), " ")).show(2)
// 按照空格" "将Description列分割成数组
// 将"WHITE HANGING ON"分割成[WHITE, HANGING, ON]

df.select(split(col("Description"), " ").alias("array_col"))
  .selectExpr("array_col[0]").show(2)
// 按照空格" "将Description列分割成数组并选择数组的第一个元素
```

```python
from pyspark.sql.functions import split
df.select(split(col("Description"), " ")).show(2)

df.select(split(col("Description"), " ").alias("array_col"))\
  .selectExpr("array_col[0]").show(2)
```

```sql
-- in SQL
SELECT SPLIT(Description, ' ') FROM dfTable;
```





## stddev_pop()
计算标准差

```
// in Scala
import org.apache.spark.sql.functions.{stddev_pop}

# in Python
from pyspark.sql.functions import stddev_pop
```


## struct()
构建结构体
- 在查询中用圆括号括起一组列来创建一个结构体

```
df.selectExpr("(Description, InvoiceNo) as complex", "*")
df.selectExpr("struct(Description, InvoiceNo) as complex", "*")
```


```scala
// in Scala
import org.apache.spark.sql.functions.struct
val complexDF = df.select(struct("Description", "InvoiceNo").alias("complex"))
complexDF.createOrReplaceTempView("complexDF")
```

```python
# in Python
from pyspark.sql.functions import struct
complexDF = df.select(struct("Description", "InvoiceNo").alias("complex"))
complexDF.createOrReplaceTempView("complexDF")
```

- 使用`.`或`getField`访问结构体中的列

```
complexDF.select("complex.Description")
complexDF.select(col("complex").getField("Description"))  // 访问结构体complexDF中的Description列
```


- 使用`.*`查询结构体中的所有值
```
complexDF.select("complex.*")
```


## to_date()
以指定的格式将字符串转换为日期数据
- 默认格式：`yyyy-MM-dd`（年-月-日）
- 需要在Java SimpleDataFormat中指定我们想要的格式

```scala
// in Scala
import org.apache.spark.sql.functions.{months_between, to_date}

dateDF.select(
  to_date(lit("2016-01-01")).alias("start"),
  to_date(lit("2017-05-22")).alias("end"))
  .select(months_between(col("start"), col("end"))).show(1)

val dateFormat = "yyyy-dd-MM"  // 指定日期格式为年-日-月
val cleanDateDF = spark.range(1).select(
  to_date(lit("2017-12-11"), dateFormat).alias("date"),
  to_date(lit("2017-20-12"), dateFormat).alias("date2"))
cleanDateDF.createOrReplaceTempView("dateTable2")
```

```python
# in Python
from pyspark.sql.functions import months_between, to_date

dateDF.select(
  to_date(lit("2016-01-01")).alias("start"),
  to_date(lit("2017-05-22")).alias("end"))\
  .select(months_between(col("start"), col("end"))).show(1)

dateFormat = "yyyy-dd-MM"  ## 指定日期格式为年-日-月
cleanDateDF = spark.range(1).select(
  to_date(lit("2017-12-11"), dateFormat).alias("date"),
  to_date(lit("2017-20-12"), dateFormat).alias("date2”))
cleanDateDF.createOrReplaceTempView("dateTable2")
```

```sql
-- in SQL
SELECT 
   to_date('2016-01-01'), 
   months_between('2016-01-01', '2017-01-01'), datediff('2016-01-01', '2017-01-01')
FROM dateTable;

SELECT to_date(date, 'yyyy-dd-MM'), to_date(date2, 'yyyy-dd-MM'), to_date(date)
FROM dateTable2;
```

## to_json()
将StructType转换为JSON字符串

```scala
// in Scala
import org.apache.spark.sql.functions.to_json
df.selectExpr("(InvoiceNo, Description) as myStruct")
  .select(to_json(col("myStruct")))
```

```python
# in Python
from pyspark.sql.functions import to_json
df.selectExpr("(InvoiceNo, Description) as myStruct")\
  .select(to_json(col("myStruct")))
```


## to_timestamp()
将字符串转换为时间戳
- 要求指定一种格式

```scala
import org.apache.spark.sql.functions.to_timestamp
val dateFormat = "yyyy-dd-MM"
cleanDateDF.select(to_timestamp(col("date”), dateFormat)).show()  // date列的日期格式为"yyyy-dd-MM"
```

```python
# in Python
from pyspark.sql.functions import to_timestamp
dateFormat = "yyyy-dd-MM"
cleanDateDF.select(to_timestamp(col("date"), dateFormat)).show()
```

```sql
-- in SQL
SELECT to_timestamp(date, 'yyyy-dd-MM'), to_timestamp(date2, 'yyyy-dd-MM')
FROM dateTable2;

SELECT CAST(TO_DATE('2020-01-01', 'yyyy-dd-MM') AS TIMESTAMP);
```


## translate()
用给定的字符替换掉列中出现的所有该字符

```scala
// in Scala
import org.apache.spark.sql.functions.translate
df.select(translate(col("Description"), "LEET", "1337"), col("Description"))
.show(2)
// Description中的所有L都被替换成1，所有E都被替换成3，所有的T都被替换成7
// 替换前：WHITE METAL LANTERN
// 替换后：WHI73 M37A1 1AN73RN
```

```python
# in Python
from pyspark.sql.functions import translate
df.select(translate(col("Description"), "LEET", "1337"),col("Description"))\
  .show(2)
```

```sql
-- in SQL
SELECT translate(Description, 'LEET', '1337'), Description FROM dfTable;
```



## rdd
### getNumPartitions
获取分区数

```scala
// in Scala
df.rdd.getNumPartitions // 1
```

```python
# in Python
df.rdd.getNumPartitions() # 1
```


# Row()
- 手动创建Row对象，必须按照该行所属的DataFrame的列顺序来初始化Row对象

```scala
// in scala
// 创建Row对象
import org.apache.spark.sql.Row
val myRow = Row("Hello", null, 1, false)
```

```python
# in python
# 创建Row对象
from pyspark.sql import Row
myRow = Row("Hello", None, 1, False)
```


## stat
### approxQuantile()
计算数据的精确分位数或近似分位数

```scala
// in Scala
val colName = "UnitPrice"
val quantileProbs = Array(0.5)
val relError = 0.05
df.stat.approxQuantile("UnitPrice"， quantileProbs， relError)
```

```python
# in Python
colName = "UnitPrice"
quantileProbs = [0.5]
relError = 0.05
df.stat.approxQuantile("UnitPrice", quantileProbs, relError)
```

### crosstab()
查看交叉列表

```scala
// in Scala or Python
df.stat.crosstab("StockCode"， "Quantity").show()
```

### freqItems()
查看频繁项对

```scala
// in Scala
df.stat.freqItems(Seq("StockCode"， "Quantity")).show()
```

```python
# in Python
df.stat.freqItems(["StockCode"， "Quantity"]).show()
```


## 反引号
当列名中包含空格或连字符等保留字时，有时需要通过使用反引号（注意是Tab键上方的反引号键，不是单引号）适当地对列名进行转义
- `withColumn`允许使用保留字来创建列（因为withColumn的第一个参数只是新列名的字符串）
- 如果我们显式地使用字符串来引用列，则可以引用带有保留字符的类（而不用转义），这个字符串会被解释成字面值，而不是表达式

```scala
// withColumn允许使用保留字来创建列
import org.apache.spark.sql.functions.expr

val dfWithLongColName = df.withColumn(
   "This Long Column-Name",  // 因为withColumn的第一个参数只是新列名的字符串
   expr("ORIGIN_COUNTRY_NAME)")
)
// 引用包含保留字的列时，需要进行转义
dfWithLongColName.selectExpr(
   "`This Long Column-Name`",
   "`This Long Column-Name` as `new col`")
.show(2)
```


# 运算符

||Scala|Python|SQL|
|:--:|:---:|:---:|:---:|
|不等于|=!=|!=|!= 或 <>|

## 不等于
- Scala中的“等于”为`===`或`equalTo()`，“不等于”为`=!=`或`not()`；
- Scala中的`=!=`不仅能比较字符串，也能比较表达式
- Python的“不等于”为`!=`
- 还可以使用下列方式（字符串形式的谓词表达式）表达“不等于”（Python或Scala都支持）  
   ```scala
   // in Scala
   df.where("InvoiceNo = 536365")
      .show(5, false)
   // 或
   df.where("InvoiceNo <> 536365")
      .show(5, false)
   ```

# 时间序列






# 配置
## spark.conf
### .sql.shuffle.partitions
- 默认情况，shuffle操作会输出200个shuffle分区
```
spark.conf.set("spark.sql.shuffle.partitions", "5")
// 限制shuffle输出分区的数量
```

### sessionLocalTimeZone
设置会话本地时区



## spark.sql
### caseSensitive
Spark默认不区分大小写，可以通过以下配置使Spark区分大小写
```sql
-- in SQL
set spark.sql.caseSensitive true
```


# 数据类型

## TimestampType
- Spark的TimestampType只支持二级精度
  - 如果要处理毫秒或微秒，需要将数据作为long类型操作才能解决该问题
  - 在强制转换为TimestampType时，任何更高的精度都被删除

目前到102页


```scala
// in scala

```


```python
# in python

```

```sql
-- in SQL

```


# 参考资料
- [Spark权威指南](https://book.douban.com/subject/30449649/)
- [Spark-SQL之DataFrame操作大全](https://blog.csdn.net/dabokele/article/details/52802150)
- [Spark四大组件](https://blog.csdn.net/Struggle99/article/details/103799524)
- []()