---
title: Data Scientist | Spark 数据源
date: 2020-09-01 10:01:14
tags: [Data Scientist, Spark]
categories: Data Scientist
math: true
mathjax: true
---

<center>data source</center>
<!--more-->

# 数据源
## 核心数据源
Spark的6大核心数据源：
- CSV
- JSON
- Parquet
- ORC
- JDBC/ODBC连接
- 纯文本文件

## 由社区创建的数据源
- Cassandra
- HBase
- MongoDB
- AWS Redshift
- XML
- 其他

# 数据源API
## Read API
读取数据的核心结构：`DataFrameReader.format(...).option("key", "value").schema(...).load()`
- `format()`：可选，默认情况下Spark将使用Parquet格式
- `option()`：配置键值对来参数化读取数据的方式

Spark数据读取使用`DataFrameReader`，通过SparkSession的read属性得到
```
Spark.read
```

```
// 例子
spark.read.format(“csv")
    .option(“mode", “FAILFAST")  // 读取模式
    .option(“inferSchema", “true")  // 使用模式推理
    .option(“path", “path/to/file(s)")  // 设置路径
    .schema(someSchema)  // 模式
    .load()
```
### 读取模式
读取模式指定当Spark遇到错误格式的记录时应采取的操作
- 默认是`permissive`

|读取模式|说明|
|:---:|:-----|
|`permissive`|遇到错误格式的记录时，将所有字段设置为null并将所有错误格式的记录放在名为`_corrupt_record`字符串列中|
|`dropMalformed`|删除包含错误格式记录的行|
|`failFast`|遇到错误格式的记录后，立即返回失败|

## Write API
写数据的核心结构：
`DataFrameWriter.format(...).option(...).partitionBy(...).bucketBy(...).sortBy(...).save()`
- `format()`：可选，默认情况下Spark将使用Parquet格式
- `option()`：配置写出数据的方式
- `PartitionBy`、`bucketBy`、`sortBy`仅适用基于文件的数据源

Spark的写数据使用`DataFrameWriter`，通过DataFrame的write属性来获取DataFrameWriter：
```
dataFrame.write
```

```scala
// in Scala
// in Scala
dataframe.write.format(“csv")
    .option(“mode", “OVERWRITE")  // 保存模式
    .option(“dateFormat", “yyyy-MM-dd")
    .option(“path", “path/to/file(s)")
    .save()
```

### 保存模式
保存模式指明如果Spark在指定目标路径发现有其他数据占用时应采取的操作。
- 默认是`errorIfExists`

|保存模式|说明|
|:----:|:-------|
|`append`|将输出文件追加到目标路径已存在的文件上或目录的文件列表|
|`overwrite`|将完全覆盖目标路径中已存在的任何数据|
|`errorIfExists`|如果目标路径已存在数据或文件，则抛出错误并返回写入操作失败|
|`Ignore`|如果目标路径已存在数据或文件，则不执行任何操作|

# 核心数据源
## CSV
逗号分隔值（CSV，comma-separated values），一种常见的文本文件格式；每行表示一条记录，用逗号分隔记录中的每个字段
- 是最难处理的文件格式之一

**CSV数据源选项**：

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: center;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center;">
      <th>读取/写入</th>
      <th>Key</th>
      <th>取值范围</th>
      <th>默认值</th>
      <th>说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5">Read</th>
      <td>escapeQuotes</td>
      <td>true, false</td>
      <td>true</td>
      <td>声明Spark是否应该转义在行中找到的引号</td>
    </tr>
    <tr>
      <td>maxColumns</td>
      <td>任意整数</td>
      <td>20480</td>
      <td>声明文件中的最大列数</td>
    </tr>
    <tr>
      <td>maxCharsPerColumn</td>
      <td>任意整数</td>
      <td>1000000</td>
      <td>声明列中的最大字符数</td>
    </tr>
    <tr>
      <td>maxMalformedLogPerPartition</td>
      <td>任意整数</td>
      <td>10</td>
      <td>设置Spark将为每个分区记录错误格式的行的最大数目，超出此数目的格式错误的记录将被忽略</td>
    </tr>
    <tr>
      <td>multiline</td>
      <td>true, false</td>
      <td>false</td>
      <td>此选项用于读取多行CSV文件，其中CSV文件中的每个逻辑行可能跨越文件本身中的多行</td>
    </tr>
    <tr>
      <th>Write</th>
      <td>quoteAll</td>
      <td>true, false</td>
      <td>false</td>
      <td>指定是否将所有值括在引号中，而不是仅转义具有引号字符的值</td>
    </tr>
    <tr>
      <th rowspan="13">Read & Write</th>
      <td>Compression 或 codec</td>
      <td>None, uncompressed, bzip2, deflate, gzip, lz4, snappy</td>
      <td>none</td>
      <td>声明Spark应该使用什么压缩编码器来读取或写入文件</td>
    </tr>
    <tr>
      <td>dateFormat</td>
      <td>任何符合Java的SimpleDataFormat式的字符串或字符</td>
      <td>yyyy-MM-dd</td>
      <td>日期类型的列的日期格式</td>
    </tr>
    <tr>
      <td>escape</td>
      <td>任意字符串字符</td>
      <td>\</td>
      <td>用于转义的字符</td>
    </tr>
    <tr>
      <td>header</td>
      <td>true, false</td>
      <td>false</td>
      <td>声明文件中的第一行是否为列的名称</td>
    </tr>
    <tr>
      <td>ignoreLeadingWhiteSpace</td>
      <td>true, false</td>
      <td>false</td>
      <td>声明是否应跳过读取值中的前导空格</td>
    </tr>
    <tr>
      <td>ignoreTrailingWhiteSpace</td>
      <td>true, false</td>
      <td>false</td>
      <td>声明是否应跳过读取值中的尾部空格</td>
    </tr>
    <tr>
      <td>inferSchema</td>
      <td>true, false</td>
      <td>false</td>
      <td>指定在读取文件时Spark是否自动推断列类型</td>
    </tr>
    <tr>
      <td>nanValue</td>
      <td>任意字符串字符</td>
      <td>NaN</td>
      <td>声明在CSV文件中表示NaN或缺失字符的字符</td>
    </tr>
    <tr>
      <td>negativeInf</td>
      <td>任意字符串或字符</td>
      <td>-Inf</td>
      <td>声明什么字符表示负无穷大</td>
    </tr>
    <tr>
      <td>nullValue</td>
      <td>任意字符串字符</td>
      <td>""</td>
      <td>声明在文件中表示null值的字符</td>
    </tr>
    <tr>
      <td>positiveInf</td>
      <td>任意字符串或字符</td>
      <td>Inf</td>
      <td>声明什么字符表示正无穷大</td>
    </tr>
    <tr>
      <td>sep</td>
      <td>任意单个字符串字符</td>
      <td>,</td>
      <td>用作每个字段和值的分隔符的单个字符</td>
    </tr>
    <tr>
      <td>timestampFormat</td>
      <td>任何符合Java的SimpleDataFormat的字符串或字符</td>
      <td>MMdd HH:mm:ss.SSSZZ</td>
      <td>时间戳类型时间戳格式</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

- 通常，Spark只会在作业执行而不是DataFrame定义时发生失败

```scala
// in Scala 
// 读取CSV文件
spark.read.format("csv")  // 指定format格式为csv
  .option("header", "true")
  .option("mode", "FALIFAST")
  .option("inferSchema", "true")
  .load("some/path/to/file.csv")

// 可以创建schema以确保文件符合我们所期望的数据
import org.apache.spark.sql.types.{StructField, StructType, StringType, LongType}

val myManualSchema = new StructType(Array(
  new StructField("DEST_COUNTRY_NAME", StringType, true),
  new StructField("ORIGIN_COUNTRY_NAME", StringType, true),
  new StructField("count", LongType, false)
))
spark.read.format("csv")
  .option("header", "true")
  .option("mode", "FAILFAST")
  .option(myManualSchema)
  .load("some/path/to/file.csv")
  .show(5)
```

```scala
// in Scala 
// 读取CSV文件
val csvFile = spark.read.format(“csv")
  .option(“header", “true").option(“mode", “FAILFAST").schema(myManualSchema)
  .load(“/data/flight-data/csv/2010-summary.csv")
// 将CSV文件内容写入TSV文件
csvFile.write.format("csv")
  .mode("overwrite")
  .option("sep", "\t")
  .save("/tmp/my-tsv-file.tsv")
// my-tsv-file.tsv实际是一个包含大量文件的文件夹
```

**制表符分隔值**（TSV，Tab-separated values）
- 每一行储存一条记录
- 每条记录的各个字段间以制表符作为分隔

## JSON
JSON（JavaScript Object Notation）
- Spark中的JSON文件指的是换行符分隔的JSON，每行必须包含一个单独的、独立的有效JSON对象
- 使用换行符分隔的JSON可以在文件末尾追加新记录
- JSON对象具有结构化信息
- 当`multiLine`为`true`时，可以将整个JSON文件作为一个JSON对象读取


**JSON数据源选项**：

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: center;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center;">
      <th>读取/写入</th>
      <th>Key</th>
      <th>取值范围</th>
      <th>默认值</th>
      <th>说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3">Read & Write</th>
      <td>Compression 或 codec</td>
      <td>None, uncompressed, bzip2, deflate, gzip, lz4, snappy</td>
      <td>none</td>
      <td>声明Spark应该使用什么压缩编码器来读取或写入文件</td>
    </tr>
    <tr>
      <td>dateFormat</td>
      <td>任何符合Java的SimpleDataFormat式的字符串或字符</td>
      <td>yyyy-MM-dd</td>
      <td>日期类型的列的日期格式</td>
    </tr>
    <tr>
      <td>timestampFormat</td>
      <td>任何符合Java的SimpleDataFormat的字符串或字符</td>
      <td>MMdd HH:mm:ss.SSSZZ</td>
      <td>时间戳类型时间戳格式</td>
    </tr>
    <tr>
      <th rowspan="8">Read</th>
      <td>allowComments</td>
      <td>true, false</td>
      <td>false</td>
      <td>忽略JSON记录中的Java/C++样式注释</td>
    </tr>
    <tr>
      <td>allowBackslashEscAPIngAny</td>
      <td>true, false</td>
      <td>false</td>
      <td>是否允许反斜杠机制接收所有字符</td>
    </tr>
    <tr>
      <td>allowNumericLeadingZeros</td>
      <td>true, false</td>
      <td>false</td>
      <td>是否允许数字中存在前导零</td>
    </tr>
    <tr>
      <td>allowSingleQuotes</td>
      <td>true, false</td>
      <td>true</td>
      <td>除双引号外，是否允许使用单引号</td>
    </tr>
    <tr>
      <td>allowUnquotedFieldNames</td>
      <td>true, false</td>
      <td>false</td>
      <td>允许不带引号的JSON字段名</td>
    </tr>
    <tr>
      <td>columnNameOfCorruptRecord</td>
      <td>任意字符串或字符</td>
      <td>spark.sql.column & NameOfCorruptRecord</td>
      <td>允许重命名permissive模式下添加的新字段，会覆盖重写</td>
    </tr>
    <tr>
      <td>multiLine</td>
      <td>true, false</td>
      <td>false</td>
      <td>允许读取非换行符分隔的JSON文件</td>
    </tr>
    <tr>
      <td>primitiveAsString</td>
      <td>true, false</td>
      <td>false</td>
      <td>将所有原始值推断为字符串类型</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

```scala
// in Scala 
// 读取换行符分隔的JSON文件
spark.read.format("json")

spark.read.format(“json")
  .option(“mode", “FAILFAST")
  .schema(myManualSchema)
  .load(“/data/flight-data/json/2010-summary.json").show(5)

// 将CSV文件内容写入JSON文件
csvFile.write.format(“json")
  .mode(“overwrite")
  .save(“/tmp/my-json-file.json")
```

- 将DataFrame写入JSON文件：每个数据分片作为一个文件写出，整个DataFrame将输出到一个文件夹；文件中的每行仍代表一个JSON对象

## Parquet文件
Parquet是一种开源的面向列的数据存储格式
- 提供了各种存储优化，尤其适合数据分析
- 提供列压缩，从而节省空间
- 支持按列读取而非读取整个文件
- 是Spark的默认文件格式
- 从Parquet文件读取比从JSON或CSV文件效率更高
- 支持复杂类型：列是一个数组、map映射、struct结构体，都可以正常读取和写入；而CSV文件无法存储数组列

- Parquet在存储数据时执行本身的schema
- 一般在读取的时候使用默认的schema

```scala
// in Scala 
spark.read.format(“parquet")

spark.read.format(“parquet")
  .load(“/data/flight-data/parquet/2010-summary.parquet").show(5)
```

- Parquet只有很少的可选项

**Parquet数据源选项**：

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: center;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center;">
      <th>读取/写入</th>
      <th>Key</th>
      <th>取值范围</th>
      <th>默认值</th>
      <th>说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Read & Write</th>
      <td>Compression 或 codec</td>
      <td>None, uncompressed, bzip2, deflate, gzip, lz4, snappy</td>
      <td>none</td>
      <td>声明Spark应该使用什么压缩编码器来读取或写入文件</td>
    </tr>
    <tr>
      <th>Read</th>
      <td>mergeSchema</td>
      <td>true, false</td>
      <td>配置值spark.sql.par.quet.mergeSchema</td>
      <td>增量地添加列到同一表/文件夹中的Parquet文件里；此选项用于启用或禁用此功能</td>
    </tr>
  </tbody>
</table>
</div>

- 写Parquet文件和读取Parquet文件，都只需指定文件的位置即可

```scala
// in Scala 
// 写Parquet文件
csvFile.write.format(“parquet")
  .mode(“overwrite")
  .save(“/tmp/my-parquet-file.parquet")
```

## ORC文件
ORC文件是为Hadoop作业设计的自描述、类型感知的列存储文件格式
- 针对大型流式数据读取进行优化
- 读取ORC文件数据时没有可选项

**ORC和Parquet有何区别？**
- 在大多数情况下，二者非常相似
- 本质区别：
  - Parquet针对Spark进行优化
  - ORC针对Hive进行优化

```scala
// in Scala 
// 读取ORC文件
spark.read.format("orc")
  .load("/data/flight-data/orc/2010-summary.orc").show(5)

// 写ORC文件
csvFile.write.format("orc")
  .mode("overwrite")
  .save("/tmp/my-json-file.orc")
```


## JDBC/ODBC连接
即从数据库读写数据

**JDBC数据源选项**：

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: center;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center;">
      <th>读取/写入</th>
      <th>Key</th>
      <th>取值范围</th>
      <th>默认值</th>
      <th>说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4">Read & Write</th>
      <td>dbtable</td>
      <td>可以使用SQL查询的FROM子句中的任何有效内容</td>
      <td></td>
      <td>表示要读取的JDBC表</td>
    </tr>
    <tr>
      <td>driver</td>
      <td></td>
      <td></td>
      <td>用于连接到此URL的JDBC驱动器的类名</td>
    </tr>
    <tr>
      <td>numPartitions</td>
      <td></td>
      <td></td>
      <td>在读取和写入数据表时，数据表可用于并行的最大分区数（决定了并发JDBC连接的最大数目）</td>
    </tr>
    <tr>
      <td>url</td>
      <td></td>
      <td></td>
      <td>表明要连接的JDBC URL，可以在URL中指定特定源的连接属性<br>如：`jdbc:postgresql://localhost/test?user=fred&password=secret`</td>
    </tr>
    <tr>
      <th rowspan="5">Read</th>
      <td>batchsize</td>
      <td></td>
      <td>1000</td>
      <td>表示JDBC批处理大小，用于指定每次写入多少条记录。</td>
    </tr>
    <tr>
      <td>createTableColumnTypes</td>
      <td>有效的Spark SQL数据类型</td>
      <td></td>
      <td>表示创建表时使用的数据库列数据类型，而不使用默认值。<br>应该使用与`CREATE TABLE`列语法相同的格式来指定数据类型信息，指定的类型应是有效的Spark SQL数据类型</td>
    </tr>
    <tr>
      <td>createTableOptions</td>
      <td></td>
      <td></td>
      <td>用于在创建表时设置特定数据库的表和分区选项</td>
    </tr>
    <tr>
      <td>isolationLevel</td>
      <td>NONE, READ_COMMITED, READ_UNCOMMITTED, REPEATABLE_READ, SERIALIZABLE</td>
      <td>READ_UNCOMMITTED</td>
      <td>表示数据库的事务隔离级别（适用于当前连接）。<br>可取值分别对应于JDBC的Connection对象定义的标准事务隔离级别。</td>
    </tr>
    <tr>
      <td>truncate</td>
      <td>true, false</td>
      <td>false</td>
      <td>待补充</td>
    </tr>
    <tr>
      <th rowspan="2">Write</th>
      <td>fetchsize</td>
      <td></td>
      <td></td>
      <td>表示JDBC每次读取多少条记录</td>
    </tr>
    <tr>
      <td>partitionColumn<br>lowerBound<br>upperBound</td>
      <td></td>
      <td></td>
      <td>如果指定了其中一个选项，则必须设置其他所有选项；此外，还必须指定`numPartitions`。<br>这些属性描述了如何在从多个worker并行读取时对表格进行划分。<br>`partitionColumn`是要分区的列，必须是整数类型。<br>`lowerBound`和`upperBound`仅用于确定分区跨度，而不用于过滤表中的行（因此表中的所有行都将被划分并返回）。</td>
    </tr>
  </tbody>
</table>
</div>

从数据库读取文件：先指定格式（format）和选项，然后加载数据

```scala
// in Scala 
val driver = "org.sqlite.JDBC"
val path = "/data/flight-data/jdbc/my-sqlite.db"
val url = s"jdbc:sqlite:/${path}"
val tablename = "flight_info"
```

```python
# in Python 
driver = "org.sqlite.JDBC"
path = "/data/flight-data/jdbc/my-sqlite.db"
url = "jdbc:sqlite:" + path
tablename = "flight_info"
```

- SQLite与其他数据库不同，SQLite只是计算机上的一个文件
- 如果是其他数据库，需要测试连接：
```scala
// in Scala 
// 测试连接
import java.sql.DriverManager
val connection = DriverManager.getConnection(url)
connection.isClosed()
connection.close()

// 如果连接成功，则可继续执行下一步
// 从SQL表中读取DataFrame
val dbDataFrame = spark.read.format("jdbc")
  .option("url", url)
  .option("dbtable", tablename)
  .option("driver", driver)
  .load()
```

- SQLite需要的配置很简单，而其他数据库需要配置更多的参数

```scala
// in Scala 
val pgDF = spark.read
  .format("jdbc")
  .option("driver", "org.postgresql.Driver")
  .option("url", "jdbc:postgresql://database_server")
  .option("dbtable", "schema.tablename")
  .option("user", "username")
  .option("password","my-secret-password").load()
```

```scala
// in Scala 
val props = new java.util.Properties
props.setProperty("driver", "org.sqlite.JDBC")
val predicates = Array(
  "DEST_COUNTRY_NAME = 'Sweden' OR ORIGIN_COUNTRY_NAME = 'Sweden'",
  "DEST_COUNTRY_NAME = 'Anguilla' OR ORIGIN_COUNTRY_NAME = 'Anguilla'")
spark.read.jdbc(url, tablename, predicates, props).show()  // 读取JDBC
spark.read.jdbc(url, tablename, predicates, props)
  .rdd.getNumPartitions  // 查看最大分区数
```

```python
# in Python 
props = {"driver":"org.sqlite.JDBC"}
predicates = [
  "DEST_COUNTRY_NAME = 'Sweden' OR ORIGIN_COUNTRY_NAME = 'Sweden'",
  "DEST_COUNTRY_NAME = 'Anguilla' OR ORIGIN_COUNTRY_NAME = 'Anguilla'"]
spark.read.jdbc(url, tablename, predicates=predicates, properties=props).show()
spark.read.jdbc(url,tablename,predicates=predicates,properties=props)\
  .rdd.getNumPartitions()
```

写入SQL数据库，只需指定URL并指定写入模式。

```scala
// in Scala 
val newPath = "jdbc:sqlite://tmp/my-sqlite.db"
csvFile.write.mode("overwrite")
  .jdbc(newPath, tablename, props)

// 查看写入结果
spark.read.jdbc(newPath, tablename, props).count()
// 追加新表
csvFile.write.mode("append").jdbc(newPath, tablename, props)
```

```python
# in Python 
newPath = "jdbc:sqlite://tmp/my-sqlite.db"
csvFile.write.jdbc(newPath, tablename, mode="overwrite", properties=props)

# 查看写入结果
spark.read.jdbc(newPath, tablename, properties=props).count()
# 追加新表
csvFile.write.jdbc(newPath, tablename, mode="append", properties=props)
```


## 文本文件
- 文本文件中的每一行将被解析为DataFrame中的一条记录，然后根据要求进行转换
- 文本文件能够充分利用原生类型（native type）的灵活性，因此很适合作为Dataset API的输入
- 读取文本文件时，只需指定类型为`textFile`即可
- 写文本文件时，需确保仅有一个字符串类型的列写出；否则，写操作将失败
- 如果在执行写操作时，同时执行某些数据分片操作，则可以写入更多的列（这些列将在要写入的文件夹中显示为目录，而不是在每个文件中存在多列）

```scala
// in Scala
// 读取文本文件 
spark.read.textFile("/data/flight-data/csv/2010-summary.csv")
  .selectExpr("split(value, ',') as rows").show()
```

# 其他
- 可以通过在写入之前空值数据分片来控制写入文件的并行度
- 可以通过控制数据分桶（bucketing）和数据划分（partitioning）来控制特定的数据布局方式

- 如果使用的是Hadoop分布式文件系统（HDFS），则如果该文件包含多个文件块，分割文件可进一步优化提高性能。同时需要进行压缩管理

- 并非所有的压缩格式都是可分割的
- 推荐采用gzip压缩格式的Parquet文件格式

- 多个执行器不能同时读取同一文件，但可同时读取不同的文件
  - 当从包含多个文件的文件夹中读取时，每个文件都将被视为DataFrame的一个分片，并由执行器并行读取，多余的文件会进入读取队列等候

- 写数据涉及的文件数量取决于DataFrame的分区数
  - 默认情况：每个数据分片都会有一定的数据写入

```
csvFile.repartition(5).write.format("csv").save("/tmp/multiple.csv")
// 会生成包含5个文件的文件夹
```

- 使用`partitionBy`进行数据划分，可以在后续读取时跳过大量的数据，只读入与问题相关的数据
  - 基于日期来划分数据最常见

```scala
// in Scala
csvFile.limit(10).write.mode("overwrite")
  .partitionBy("DEST_COUNTRY_NAME")
  .save("/tmp/partitioned-files.parquet")
```

**数据分桶**：具有相同桶ID（哈希分桶的ID）的数据将放置到一个物理分区中；可以避免在后续读取数据时进行shuffle

```scala
// in Scala 
val numberBuckets = 10
val columnToBucketBy = "count"

csvFile.write.format("parquet").mode("overwrite")
  .bucketBy(numberBuckets, columnToBucketBy)
  .saveAsTable("bucketedFiles")
```

- 数据分桶仅支持Spark管理的表

# 参考资料
- [Spark权威指南](https://book.douban.com/subject/30449649/)
