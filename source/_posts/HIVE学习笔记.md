---
title: HIVE学习笔记
date: 2020-06-22 12:57:53
tags: [HIVE,数据库,SQL]
categories: 数据库
mathjax: true
toc: true
hide: true
notshow: true
---


<center></center>
<!--more-->

# HIVE
- HIVE最适合于数据仓库应用程序
- 用户可以通过查询生成新表或将查询结果导入到文件中

**HIVE不支持的**
- HIVE不是一个完整的数据库
- HIVE不支持记录级别的更新、插入或删除操作
- HIVE不支持事务
- HIVE不支持OLTP(联机事务处理，Online Transaction Processing)所需的关键功能，而更接近成为一个OLAP（联机分析处理，Online Analytical Processing）工具
- HiveQL不符合ANSI SQL标准，和Oracle、MySQL、SQL Server支持的常规SQL方言在很多方面存在差异
 - HiveQL和MySQL提供的SQL方言最接近

# 基本
## 命名空间

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
      <th>命名空间</th>
      <th>使用权限</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>hivevar</th>
      <td>可读、可写</td>
      <td>用户自定义变量（HIVE v0.8.0以上）</td>
    </tr>
    <tr>
      <th>hiveconf</th>
      <td>可读、可写</td>
      <td>Hive相关的配置属性</td>
    </tr>
    <tr>
      <th>system</th>
      <td>可读、可写</td>
      <td>Java定义的配置属性</td>
    </tr>
    <tr>
      <th>env</th>
      <td>只可读</td>
      <td>Shell环境定义的环境变量</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

- 用户可以在查询中引用变量。Hive会先使用变量值替换掉查询的变量引用，然后才会将查询语句提交给查询处理器
- 和hivevar变量不同，用户必须使用 `system:`或`env:`前缀来指定系统属性和环境变量




## 数据类型

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
      <th>数据类型</th>
      <th>长度</th>
      <th>例子</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>TINYINT</th>
      <td>1字节、有符号整数</td>
      <td>20</td>
    </tr>
    <tr>
      <th>SMALINT</th>
      <td>2字节、有符号整数</td>
      <td>20</td>
    </tr>
    <tr>
      <th>INT</th>
      <td>4字节、有符号整数</td>
      <td>20</td>
    </tr>
    <tr>
      <th>BIGINT</th>
      <td>8字节、有符号整数</td>
      <td>20</td>
    </tr>
    <tr>
      <th>BOOLEAN</th>
      <td>布尔类型</td>
      <td>TRUE 或 FALSE</td>
    </tr>
    <tr>
      <th>FLOAT</th>
      <td>单精度浮点数</td>
      <td>3.14159625</td>
    </tr>
    <tr>
      <th>DOUBLE</th>
      <td>双精度浮点数</td>
      <td>3.14159625</td>
    </tr>
    <tr>
      <th>STRING</th>
      <td>字符序列；可以指定字符集<br>可以使用单引号或双引号</td>
      <td>'Hello, World!'<br>"print something"</td>
    </tr>
    <tr>
      <th>TIMESTAMP</th>
      <td>整数、浮点数、字符串</td>
      <td>1327882394（Unix新纪元秒）<br>
      1327882394.123456789（Unix新纪元秒+纳秒数）<br>
      '2012-02-03 12:34:56.123456789'（JDBC所兼容的java.sql.Timestamp时间格式）</td>
    </tr>
    <tr>
        <th>BINARY</th>
        <td>字节数组</td>
        <td></td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

### INT
`CAST(s AS INT)`
将一个字符串的列转换为整型


### TIMESTAMP
数据类型`TIMESTAMP`的值可以是整数，也可以是浮点数，也可以是字符串
- 整数：距离UNIX新纪元时间（1970年1月1日午夜12点）的秒数
- 浮点数：距离UNIX新纪元时间（1970年1月1日午夜12点）的秒数，精确到纳秒（小数点后保留9位小数）
- 字符串：JDBC约定的时间字符串格式 `YYYY-MM-DD hh:mm:ss.fffffffff`

### TIMESTAMPS
`TIMESTAMPS`表示的是UTC时间。

### 比较
如果用户在查询中将一种整型类型的值和另一种整型类型的值作对比，则HIVE会隐式地将类型转换未两个整型类型中值较大的那个类型。
- 如果用户在查询中将一个FLOAT类型的列与一个DOUBLE类型的列作对比，则HIVE会隐式地将FLOAT类型转换为DOUBLE类型然后进行比较。
  - FLOAT类型的0.2实际是0.2000001，而DOUBLE类型的0.2是0.200000000001
  - 在对数据进行比较时，可使用`CAST(data AS FLOAT)`指定数据类型，再进行比较，防止出错
  - 与钱相关的数都避免使用浮点数


## 集合数据类型
### ARRAY
一组具有相同类型和名称的变量的集合。
- 元素：这些变量称为数组的元素
- 每个数组元素都有一个编号，编号从0开始
- 应用一个不存在的元素将会返回NULL
- 提取出来的STRING数据类型的值不再加引号

```json
Array('John', 'Doe')
```
其中`'Doe'`的应用应为`数组名[1]`。

### MAP
- MAP是一组键值对元组集合，使用数组表示法访问元素（使用键值索引而不是整数索引）

```json
map('first', 'John', 'last', 'Doe')
```

键->值对的对应关系如：
- 'first' -> 'Jhon'
- 'last' -> 'Doe'
通过`字段名['last']`可以获取元素`'Doe'`

- HIVE中并没有键的概念（？？），但是用户可以对表建立索引


### STRUCT
和C语言中的struct或者“对象”类似，都可以通过“点”符号访问元素内容

```sql
CREATE TABLE employees(
  name STRING,
  salary FLOAT,
  subordinates ARRAY<STRING>,
  deductions MAP<STRING, FLOAT>,
  address STRUCT<street: STRING, city: STRING, state: STRING, zip: INT>
);
-- 若要访问address中的city字段，则应 address.city
```


```js
{
  'store': {
    'fruit': [
      {
        'weight': 8,
        'type': 'apple'
      },
      {
        'weight': 9,
        'type': 'pear'
      }
    ],
    'bicycle': {
      'price': 19.95,
      'color': 'red'
    }
  },
  'email': '12345678@163.com',
  'owner': 'Jack'
}
```
假设上述JSON数据存储在字段s中，则若要读取`apple`的`weight`，则应 
```sql
GET_JSON_OBJECT(s, '$.store.fruit[0].weight')
-- 返回 8
```

## 正则表达式
- 可以使用正则表达式来选择想要的列：
```sql
-- 从stocks表中选择symbol列和所有列名以price作为前缀的列
SELECT 
  symbol
  , `price.*`  -- 注意是反引号，不是普通的单引号
FROM stocks;

```


## 字段分隔符


70页


## 数据库
- 如果没有显式指定数据库，则会使用默认的数据库default



## 表
HIVE中的表分为两种：
1. 管理表，也称内部表
2. 外部表

- 用户可在`DESCRIBE EXTENDED table_name`语句的输出中查看到该表的类型（管理表MANAGED_TABLE，外部表EXTERNAL_TABLE）
- Hive没有临时表的概念


### 管理表
- Hive默认将管理表的数据存储在由配置项`hive.metastore.warehouse.dir`所定义的目录的子目录下
- 删除一个管理表时，会删除这个表中的数据
- 管理表不方便和其他工作共享数据
- 对于管理表，用户可以对一张存在的表进行表结构复制（仅复制表结构，不复制数据）

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS db_name.table_name_1
LIKE db_name.table_name_2
LOCATION '/path/to.data';
```
其中，
- 如果`EXTERNAL`关键词省略且源表是外部表，则生成的新表也为外部表
- 如果`EXTERNAL`关键词省略且源表是管理表，则生成的新表也为管理表
- 如果有`EXTERNAL`关键词而源表是管理表，则生成的新表为外部表


### 外部表
- 删除外部表，并不会删除掉该份数据，但是描述表的元数据信息会被删除掉
- 有些HiveQL语法结构并不适用于外部表

### 分区表
- 在WHERE子句中增加谓词来按照分区值进行过滤时，这些谓词被称为**分区过滤器**
- 内部表、外部表都可以使用分区

## 存储格式
- Hive的默认存储格式是：文本文件格式
 - 这个可以通过可选的子句 `STORED AS TEXTFILE`显式指定
 - 也可以在创建表时指定各种各样的分隔符
- 除了TEXTFILE, SEQUENCEFILE, RCFILE外，用户还可以指定第三方的输入和输出格式以及SerDe——允许用户自定义Hive本身不支持的其他广泛的文件格式

### TEXTFILE
- Hive的默认存储格式

### SEQUENCEFILE
- SEQUENCEFILE和RCFILE都是使用二进制编码和压缩（可选）来优化磁盘空间使用及I/O带宽性能的


### RCFILE


# 运算符
## 算术运算符
- Hive支持所有典型的算术运算符
- 如果数据类型不同，则两种类型中值范围较小的那个数据类型将转换为其他范围更广的数据类型
- Hive遵循的是底层Java中数据类型的规则，当数据溢出或下溢发生时，计算结果不会自动转换为更广泛的数据类型

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
      <th>运算符</th>
      <th>类型</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A+B</th>
      <td>数值</td>
      <td>A和B相加</td>
    </tr>
    <tr>
      <th>A-B</th>
      <td>数值</td>
      <td>A减去B</td>
    </tr>
    <tr>
      <th>A * B</th>
      <td>数值</td>
      <td>A和B相乘</td>
    </tr>
    <tr>
      <th>A/B</th>
      <td>数值</td>
      <td>A除以B<br>如果能整除，则返回商数（商数是一个整数）</td>
    </tr>
    <tr>
      <th>A%B</th>
      <td>数值</td>
      <td>A除以B的余数</td>
    </tr>
    <tr>
      <th>A&B</th>
      <td>数值</td>
      <td>A和B按位取与</td>
    </tr>
    <tr>
      <th>A|B</th>
      <td>数值</td>
      <td>A和B按位取或</td>
    </tr>
    <tr>
      <th>A^B</th>
      <td>数值</td>
      <td>A和B按位取亦或</td>
    </tr>
    <tr>
      <th>~A</th>
      <td>数值</td>
      <td>A按位取反</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>


## 谓词操作符
- 可用于`JOIN ... ON`和`HAVING`语句中

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
      <th>操作符</th>
      <th>支持的数据类型</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A = B</th>
      <td>基本数据类型</td>
      <td>如果A等于，则返回true；<br>否则，返回false</td>
    </tr>
    <tr>
      <th>A <> B 或 A != B</th>
      <td>基本数据类型</td>
      <td>A或B为NULL，则返回NULL；<br>如果A不等于B，则返回true；<br>否则，返回false</td>
    </tr>
    <tr>
      <th>A < B</th>
      <td>基本数据类型</td>
      <td>A或B为NULL，则返回NULL；<br>如果A小于B，则返回true；<br>否则，返回false</td>
    </tr>
    <tr>
      <th>A <= B </th>
      <td>基本数据类型</td>
      <td>A或B为NULL，则返回NULL；<br>如果A小于或等于B，则返回true；<br>否则，返回false</td>
    </tr>
    <tr>
      <th>A > B</th>
      <td>基本数据类型</td>
      <td>A或B为NULL，则返回NULL；<br>如果A大于B，则返回true；<br>否则，返回false</td>
    </tr>
    <tr>
      <th>A >= B</th>
      <td>基本数据类型</td>
      <td>A或B为NULL，则返回NULL；<br>如果A大于或等于B，则返回true；<br>否则，返回false</td>
    </tr>
    <tr>
      <th>A [NOT] BETWEEN B AND C</th>
      <td>基本数据类型</td>
      <td>如果A、B、C任一为NULL，则返回NULL；<br>如果A的值大于或等于B且小于或等于C，则返回true；<br>否则，返回false。<br>NOT关键字取反效果</td>
    </tr>
    <tr>
      <th>A IS NULL</th>
      <td>所有数据类型</td>
      <td>如果A等于NULL，则返回true;<br>否则，返回false</td>
    </tr>
    <tr>
      <th>A IS NOT NULL</th>
      <td>所有数据类型</td>
      <td>如果A不等于NULL，则返回true；<br>否则，返回false</td>
    </tr>
    <tr>
      <th>A [NOT] LIKE B</th>
      <td>STRING类型</td>
      <td>B是一个正则表达式。
      <br>如果A与正则表达式B匹配，则返回true；<br>否则，返回false。<br>NOT关键字取反效果</td>
    </tr>
    <tr>
      <th>A RLIKE B<br>A REGEXP B</th>
      <td>STRING类型</td>
      <td>B是一个正则表达式。<br>如果A与正则表达式B匹配，则返回true；<br>否则，返回false。<br>匹配使用的是JDK中的正则表达式实现，正则规则应该依据JDK的正则规则。</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>


# 语法
## -S
开启静默模式，可将输出结果中的`OK`和`Time taken`等行以及其他一些无关紧要的输出信息


## alter
大多数的表属性可以通过`ALTER TABLE`语句进行修改。
- `ALTER TABLE`会修改元数据，但不会修改数据本身

### 重命名表
```sql
 ALTER TABLE table_old_name RENAME TO table_new_name;
```

### 增加表分区
```sql
-- 增加表分区
ALTER TABLE table_name ADD IF NOT EXISTS
PARTITION (year=2020, month=7, day=15) LOCATION '/logs/2020/07/15';
```

- HIVE v0.8.0及以上版本，可在同一个查询中同时增加多个分区。

### 修改表分区
修改某个分区的路径：
```sql
ALTER TABLE table_name PARTITION(year=2020, month=7, day=15) SET LOCATION 's3n://ourbucket/logs/2020/07/15';
```

- 该命令不会将数据从旧的路径转移走，也不会删除旧的数据

### 删除表分区
删除某个表分区：
```sql
ALTER TABLE table_name DROP IF EXISTS PARTITION(year=2020, month=7, day=15);
```

- 对于管理表，即使是使用`ALTER TABLE ... ADD PARTITION`语句增加的分区，分区内的数据也会同时和元数据信息一起被删除
- 对于外部表，分区内数据不会被删除


### 修改表属性
`SET TBLPROPERTIES`

- 用户可以增加附加的表属性或修改已经存在的属性
- 但用户无法删除属性

```sql
ALTER TABLE table_name SET TBLPROPERTIES (
  'notes' = 'The process id is no longer captured; this column is always NULL'
);
```


### 修改列
`CHANGE COLUMN`

对某个字段进行重命名，修改其位置、类型或注释：
```sql
ALTER TABLE table_name
CHANGE COLUMN old_col_name new_col_name INT  -- 重命名old_col_name为new_col_name，且数据类型为INT
COMMENT 'Comment for Column new_col_name'  -- 修改该列的注释
AFTER another_column;  -- 将new_col_name的位置修改到another_column列的后面
```

### 增加列
`ADD COLUMNS`

在分区字段之前增加新的字段到已有的字段后：
```sql
ALTER TABLE table_name ADD COLUMNS (
  col_name_1 STRING COMMENT 'Application name',
  col_name_2 LONG COMMENT 'The current session id'
);
-- 其中 COMMENT 子句是可选的
```

### 删除/替换列
`REPLACE COLUMNS`

```sql
ALTER TABLE table_name REPLACE COLUMNS (
  col_name_1  INT     COMMENT 'comment 1',
  col_name_2  STRING  COMMENT 'comment 2',
  col_name_3  STRING  COMMENT 'comment 3'
);
-- 以上语句移除了表table_name的所有字段并重新制定了以上3个新的字段
-- 只有表的元数据信息改变了
```

- `REPLACE`语句只能用于使用了如下两种内置SerDe模块的表：
  - DynamicSerDe
  - MetadataTypedColumnsetSerDe

### 修改存储属性
`SET FILEFORMAT`

```sql
ALTER TABLE table_name
PARTITION (year=2020, month=7, day=15)
SET FILEFORMAT SEQUENCEFILE;
-- 将存储格式修改为SEQUENCEFILE
```

### 修改SerDe属性
用户可以指定一个新的SerDe，并为其指定SerDe属性，或修改已存在的SerDe的属性

```sql
ALTER TABLE table_using_JSON_storage
SET SERDE 'com.example.JSONSerDe'
WITH SERDEPROPERTIES (
  'prop1' = 'value1',
  'prop2' = 'value2'
);
-- 使用一个名为com.example.JSONSerDe的Java类来处理记录使用JSON编码的文件
-- SERDEPROPERTIES中的属性会被传递给SerDe模块（即com.example.JSONSerDe这个Java类）
-- 属性名和属性值都应当是带引号的字符串
```

向已经存在着的SerDe增加新的SERDEPROPERTIES属性：
```sql
ALTER TABLE table_using_JSON_storage
SET SERDEPROPERTIES (
  'prop3' = 'value3',
  'prop4' = 'value4'
);
```

## as
给列起别名，或给中间表起别名

## case...when...then
`CASE ... WHEN ... THEN`语句与if条件语句类似，用于处理单个列的查询结果
```sql
SELECT
  name,
  salary,
  CASE
    WHEN salary < 50000.0 THEN 'low'
    WHEN salary >= 50000.0 AND salary < 70000.0 THEN 'middle'
    WHEN salary >= 70000.0 AND salary < 100000.0 THEN 'high'
    ELSE 'very high'
  END AS bracket
FROM employees;
```


## cluster by
- `CLUSTER BY`语句剥夺了`SORT BY`的并行性，可以实现输出文件的数据是全局排序的

```sql
SELECT s.ymd, s.symbol, s.price_close
FROM stocks s
DISTRIBUTE BY s.symbol
SORT BY s.symbol;
-- 等价于
SELECT s.ymd, s.symbol, s.price_close
FROM stocks s
CLUSTER BY s.symbol;
```



## comment
添加描述信息(备注).

```sql
CREATE DATABASE db_name_1
COMMENT 'Some comments for database db_name_1.';
-- 在创建数据库时添加备注，方便后续他人查看数据库的说明
```


## create

### create database
创建数据库：

```sql
CREATE DATEABASE db_name;
-- 如果该数据库db_name已存在，则会报错
-- 避免报错：
CREATE DATABASE IF NOT EXISTS db_name;
```

### create table

- 默认情况下，HIVE总是将创建的表的目录放置在这个表所属的数据库目录之后
- 但default库在`/user/hive/warehouse`下没有对应一个数据库目录，default库中的表目录会直接位于`/user/hive/warehouse`目录之后（除了用户明确指定为其他路径）

在创建表时，可以直接拷贝一张已经存在的表的表模式：
```sql
CREATE TABLE IF NOT EXISTS db_name_1.tbl_2
LIKE db_name_1.tbl_1;
```



## describe
- 查看数据库的描述信息：

```sql
DESCRIBE DATABASE db_name_1;
-- 则会返回：
-- Some comments for database db_name_1.
-- 以及数据库所在的文件目录位置路径
```

- 查看某一列的信息：
```sql
DESCRIBE db_name.table_name.col_name;
```

### extended
用户可以为数据库增加一些和其相关的键值对属性信息，通过下列语句可以显示出这些信息：

```sql
-- 创建数据库时增加一些和其相关的键值对属性信息
CREATE DATABASE db_name_1
WITH DBPROPERTIES ('creator' = 'Mark Moneybags', 'date' = '2020-07-12');

-- 显示那些信息
DESCRIBE DATABASE EXTENDED db_name_1;
db_name_1 hdfs://master-server/user/hive/warehouse/db_name_1.db
{date=2020-07-12, creator=Mark Moneybags};
```

`DESRIBE EXTENDED table_name`也会显示出分区键。

### formatted
显示数据库的信息，输出内容详细且可读性强

```sql
DESCRIBE DATABASE FORMATTED db_name_1;
```

## distinct

- 在Hive中可以实现**去重**的命令：
  - `DISTINCT`
  - `GROUP BY`
  - `ROW_NUMBER`：排序后再取topN

## distribute by
`DISTRIBUTE BY`控制map的输出在reducer中是如何划分的
- 默认情况下，MapReduce计算框架会依据map输入的键计算相应的哈希值，然后按照得到的哈希值将键-值对均匀分发到多个reducer中去
- Hive要求`DISTRIBUTE BY`语句要写在`SORT BY`语句之前
- `DISTRIBUTE BY ... SORT BY`语句剥夺了`SORT BY`的并行性，可以实现输出文件的数据是全局排序的


## drop
删除数据库：
```sql
DROP DATABASE IF EXISTS db_name_1;
```

- HIVE不允许用户删除一个包含表的数据库
 - 或，先删除数据库中的表，再删除数据库
 - 或，在删除命令的最后加上关键字`CASCADE`——可以使HIVE自行先删除数据库中的表

```sql
DROP DATABASE IF EXISTS db_name_1 CASCADE;
```

删除数据表:

```sql
DROP TABLE IF EXISTS table_name;
```

- 若删除管理表，则表的元数据信息和表内的数据都会被删除。
- 若删除外部表，则表的元数据信息会被删除，但是表中的数据不会被删除。
- 如果用户开启了Hadoop回收站功能（默认是关闭的），则数据将会被转移到用户在分布式文件系统中的用户根目录下的.Trash目录（即HDFS中的`/user/$USER/.Trash`目录）
 - 开启该功能，只需将配置属性`fs.trash.interval`的值（单位为分钟）设置为一个合理的正整数即可


## group by
通常与聚合函数一起使用，按照一个或多个列对结果进行分组，然后对每个组进行聚合操作。

## having
对`GROUP BY`语句产生的分组进行条件过滤

## join
- 大多数情况下，Hive会对每对JOIN连接对象启动一个MapReduce任务
- Hive总是按照从左到右的顺序连接表
- 对3个或者更多个表进行JOIN连接时，如果每个ON子句都使用相同的连接键，则只会产生一个MapReduce任务
- 为了优化HiveQL执行性能，应保证连续查询中的表的大小从左到右是依次增加的

### on
`ON`子句指定两个表之间数据进行连接的条件
- 标准SQL支持对连接关键词进行非等值连接
- Hive不支持连接关键词非等值连接
- Hive不支持在`ON`子句中使用`OR`谓词


### inner join
只有进行连接的两个表中都存在与连接标准相匹配的数据才会被保留下来。

### left join
左表中符合WHERE子句中的所有记录将会被返回；JOIN操作符右边表中如果没有符合ON后面连接条件的记录时，则从右边表指定选择的列的值将会是NULL

### outer join
外连接
- 外连接会忽略掉分区过滤条件

### right outer join
右外连接，会返回右边表中符合WHERE语句的记录；左表中匹配不上的字段值用NULL代替

### full outer join
完全外连接，将会返回所有表中符合WHERE语句条件的所有记录；如果任一表的指定字段没有符合条件的值的话，则使用NULL值替代

### left semi join
左半开连接，返回左边表的记录且这些记录对于右边表满足ON语句中的判定条件。
- `SELECT`和`WHERE`语句中不能引用到右边表中的字段
- Hive不支持右半开连接`RIGHT SEMI JOIN`
- `SEMI-JOIN`通常比`INNER JOIN`高效

```sql
-- SQL支持，但Hive不支持：
SELECT s.ymd, s.price_close FROM stocks s
WHERE s.yms, s.symbol IN 
  (SELECT d.ymd, d.symbol FROM dividends d);

-- 等价于Hive中的LEFT SEMI JOIN：
SELECT s.ymd, s.symbol, s.price_close
FROM stocks s
LEFT SEMI JOIN dividends d 
ON s.ymd=d.ymd AND s.symbol=d.symbol;
```

### map-side join
如果所有表中只有一张是小表，则可以在最大的表通过mapper的时候将小表完全放到内存中。
- Hive可以在map端执行连接过程（称为map-side JOIN）


## like
使用正则表达式匹配字符串
- `%`：匹配任意字符
  - `%x`：表示以x为结尾的字符串
  - `x%`：表示以x为开头的字符串
  - `%x%`：表示包含x的字符串，x可以位于开头、结尾或字符串中间
- `_`：下划线匹配单个字符


## limit
限制返回的行数



## location
创建数据库时修改数据库的存储位置

```sql
CREATE DATABASE db_name1
LOCATION '/my/preferred/directory';
```

## order by
`ORDER BY`会对查询结果集执行一个全局排序
- 会有一个所有的数据都通过一个reducer进行处理的过程
- 可以使用`SORT BY`在每个reducer中对数据进行排序（执行局部排序），可以保证每个reducer的输出数据都是（局部）有序的
- 可以指定任意期望进行排序的字段
  - 默认升序排列`ASC`
  - 若要按降序排列，则应在字段后添加`DESC`关键字

## partitioned

一般按照分区存储、管理表

```sql
CREATE TABLE employees (
  name          STRING,
  salary        FLOAT,
  subordinates  ARRAY<STRING>,
  deductions    MAP<STRING, FLOAT>,
  address       STRUCT<street: STRING, city: STRING, state: STRING, zip: INT>
)
PARTITIONED BY (country STRING, state STRING);
-- 按照country、state进行分区管理
-- country是第一级细分
-- state是第二级细分
```

`SHOW PARTITIONS table_name`：查看数据表中存在的所有分区。

```sql
SHOW PARTITIONS employees;

-- Country=CA/state=AB
-- Country=CA/state=BC
-- ...
```

`SHOW PARTITIONS table_name PARTITION(country='US')`：查看数据表中的某个一级细分下的所有分区。

```sql
SHOW PARTITIONS employees PARTITION(country='US');
SHOW PARTITIONS employees PARTITION(country='US', state='AK');
```


## rlike
通过Java的正则表达式匹配字符串
- 字符串中的点号`.`表示和任意的字符匹配
- 字符串中的星号`*`表示重复“左边的字符串”零次到无数次
- `(x|y)`表示和x或y匹配

```sql
SELECT * FROM employees
WHERE address.street RLIKE '.*(Chicago|Ontario).*';
-- 查找住址的街道名称中含有单词Chicago或Ontario的所有记录
-- 等价于
SELECT * FROM employees
WHERE address.street LIKE '%Chicago%' 
   OR address.street LIKE '%Ontario%';
```

更多Java正则表达式可见：
- [java.util.regex.Class Pattern](https://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html)
- [正则表达式手册](https://tool.oschina.net/uploads/apidocs/jquery/regexp.html)
- [正则表达式参考手册（JavaScript版本）](https://harttle.land/2016/07/18/intro-to-regexp.html)

## set
- 可用于给变量赋新的值


## show
- 查看Hive中所包含的数据库：

```sql
SHOW DATABASES;
```

- 显示当前数据库下的所有表：

```sql
SHOW TABLES;
```

- 显示指定数据库下的所有表：

```sql
SHOW TABLES IN db_name_2;
```

也可使用正则表达式筛选表名：

```sql
SHOW TABLES 'empl.*';
-- 列出所有以empl开头的所有表名
```

- 显示数据表中存在的所有分区

```sql
SHOW PARTITIONS table_name
```

## sort by
在每个reducer中对数据进行排序
- 排序结果是局部有序的
- Hive要求`DISTRIBUTE BY`语句要写在`SORT BY`语句之前


## where
WHERE语句用于过滤条件
- 不能在WHERE语句中使用列别名

## union all
可以将2个或多个表进行合并。
- 每一个UNION子查询都必需具有相同的列，且对应的每个字段的字段类型必须一致
- 也可用于同一个源表的数据合并

```sql
SELECT log.ymd, log.level, log.message
FROM 
(
  SELECT l1.ymd, l1.level, l1.message, 'Log1' AS source
  FROM log1 l1

  UNION ALL

  SELECT l2.ymd, l2.level, l2.message, 'Log2' AS source
  FROM log1 l2
) log
SORT BY log.ymd ASC;
```



## use
用于将某个数据库设置为用户当前的工作数据库

```sql
USE db_name_2;
```


## 数据函数
82页

## 聚合函数
85页


## 表生成函数
86页


# 数据操作
- Hive没有行级别的数据插入、数据更新和删除操作
- 数据插入方式有：
  - 装载数据文件到目标目录
  - INSERT INTO/OVERWRITE
  - 创建表并将查询结果载入该表

## 数据插入
### load data
装载数据到管理表：
```sql
LOAD DATA LOCAL INPATH '${env:HOME}/california-employees'
OVERWRITE INTO TABLE table_name
PARTITION (country = 'US', state = 'CA');
```

- 如果分区目录不存在，则会先创建分区目录，再将数据拷贝到该目录下
- 如果目标表是非分区表，则PARTITION子句被忽略
- 通常情况下，指定的路径应该是一个目录，而不是单个独立的文件
- `INPATH`子句中的文件路径下不可以包含任何文件夹
- Hive不会验证用户转载的数据和表的模式是否匹配
- Hive会验证文件格式是否与表结构定义一致
  - 如果表的存储格式是SEQUENCEFILE，则装载进去的文件也应该是SEQUENCEFILE格式

#### local 
- 如果使用了`LOCAL`关键字，则这个路径应该为本地文件系统路径，数据将被拷贝到目标位置
- 如果省略了`LOCAL`关键字，则这个路径应该是分布式文件系统中的路径，数据是从这个路径转移到目标位置的（因为用户在分布式文件系统中可能并不需要重复的多份数据文件拷贝）
 - 此时要求源文件和目标文件以及目录应该在同一个文件系统中
 - 用户不可以使用`LOCAL DATA`语句将数据从一个集群的HDFS中转移到另一个集群的HDFS中

#### overwrite
- 如果用户指定了`OVERWRITE`关键字，则目标文件夹中之前的数据将会被先删除
- 如果没有指定`OVERWRITE`关键字，则仅仅会把新增的文件增加到目标文件夹中而不会删除之前的数据
  - 如果目标文件夹中已经存在和装载的文件同名的文件，则旧的同名文件将会被覆盖重写（“之前的文件名_序列号”）

### insert
通过查询语句向表中插入数据：

```sql
INSERT OVERWRITE TABLE table_name
PARTITION (country = 'US', state = 'OR')
SELECT * FROM another_table se
WHERE se.cnty = 'US' AND se.et = 'OR';
```

- 如果没有使用`OVERWRITE`关键字或`INTO`关键字，则将以追加的方式写入数据，而不会覆盖掉之前已经存在的内容

```sql
-- 为三个州创建表分区
-- 将数据写入目标表的多个分区
-- 静态分区
FROM another_table se
INSERT OVERWRITE TABLE table_name
  PARTITION (country = 'US', state = 'OR')
  SELECT * WHERE se.cnty = 'US' AND se.st = 'OR'
INSERT OVERWRITE TABLE table_name
  PARTITION (country = 'US', state = 'CA')
  SELECT * WHERE se.cnty = 'US' AND se.st = 'CA'
INSERT OVERWRITE TABLE table_name
  PARTITION (country = 'US', state = 'IL')
  SELECT * WHERE se.cnty = 'US' AND se.st = 'IL'
-- 这里可以混用 INSERT OVERWRITE 句式和 INSERT INTO 句式
```

动态分区：
```sql
INSERT OVERWRITE TABLE table_name
PARTITION (country, state)
SELECT ..., se.cnty, se.st
FROM another_table se;
-- Hive根据`SELECT`语句中最后2列来确定分区字段`country`和`state`的值
```

- 源表字段值和输出分区值之间是根据位置来匹配的，而不是根据字段命名
- 可以混合使用动态分区和静态分区
```sql
INSERT OVERWRITE TABLE table_name
PARTITION (country = 'US', state)
SELECT ..., se.cnty, se.st
FROM another_table se
WHERE se.cnty = 'US';
```

- 静态分区键必须出现在动态分区键之前

### 创建表并插入数据
在单个查询语句中创建表并载入数据：
```sql
CREATE TABLE table_name
AS 
SELECT 
  name
  , salary
  , address
FROM employees se
WHERE se.state = 'CA';
```

- 该功能（在单个查询语句中创建表并载入数据）不适用于外部表

## 导出数据
- 如果不需要更改数据文件格式，则可简单地拷贝文件夹或文件即可
```shell
hadoop fs -cp source_path target_path
```

- 可用`INSERT ... DIRECTORY ...`导出数据:

```sql
INSERT OVERWRITE LOCAL DIRECTORY '/tmp/ca_employees'
SELECT 
  name
  , salary
  , address
FROM employees
WHERE se.state = 'CA';
-- 其中指定路径也可写成全URL路径（如hdfs://master-server/tmp/ca_employees
```



# 其他内置函数
## ascii()
`ASCII(STRING s)`：返回字符串s中首个ASCII字符的整数值
- 返回值是STRING

## base64()
`BASE64(BINARY bin)`：将二进制值bin转换为基于64位的字符串
- BASE64字符表：ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
- 返回值是STRING

## binary()
将输入的值转换成二进制值（Hive v0.12.0新增）
```sql
BINARY(STRING s)
BINARY(BINARY b)
```
- 返回值是BINARY

## cast(... as ...)
`CAST(<expr> AS <type>)`：将expr转换成type类型；如果转换过程失败，则会返回NULL
```sql
CAST(col_1 AS INT) -- 将col_1列转换为INT类型
CAST(col_2 AS FLOAT)  -- 将字段的类型转换为FLOAT
-- 将字段的类型转换为FLOAT，推荐使用ROUND()或FLOOR()，而不是CAST(... AS FLOAT)
CAST(col_3 AS STRING)
CAST(col_4 AS DOUBLE)
```

## collect_list()
将多个数据聚合成列表存在一个字段里
- 不去重
- 需要与`group by`结合使用

## collect_set()
将多个数据聚合成集合存在一个字段里
- 去重
- 需要与`group by`结合使用

## concat()
将多个字符串拼接成一个字符串
```sql
CONCAT('abc', 'defg')
-- 结果为 abcdefg
```
- 返回值是STRING

## concat_ws
concat with sep，拼接字符串
`CONCAT_WS(STRING seperator, BINARY s1, STRING s2, ...)`：与`concat`类似，不过是使用指定的分隔符进行拼接
- 返回值是STRING

## context_ngrams
`CONTEXT_NGRAMS(ARRAY<ARRAY<STRING>>, ARRAY<STRING>, INT k, INT pf)`：和ngrams类似，从每个外层数组的第二个单词数组来查找前k个字尾
（不理解）

## date_add
`DATE_ADD(STRING startdate, INT days)`：为开始时间`startdate`增加`days`天
- 返回值是STRING

## datediff
`DATEDIFF(STRING enddate, STRING startdate)`：计算开始时间`startdate`到结束时间`enddate`相差的天数
- 返回值是INT
```sql
DATEDIFF('2020-03-03', '2020-01')
-- 2
```

## date_format
将时间字符串转换为指定的日期格式
- 可以识别到“年”、“月”等一级的时间

```sql
DATE_FORMAT('2020-09-01 15:34:03', 'y')
-- 2020

DATE_FORMAT('2020-09-01 15:34:03', 'yyyy-MM')
-- 2020-09

DATE_FORMAT('2020-09-01', 'yyyy-MM')
-- 2020-09

DATE_FORMAT('2020-09-01 15:34:03', 'yyyy-MM-dd')
--2020-09-01

-- 当月第一天
date_format('2021-08-28', 'yyyy-MM-01')
-- 2021-08-01

-- 上个月最后一天
date_sub(date_format('2021-08-28', 'yyyy-MM-01'), 1)
```


## date_sub
`DATE_SUB(STRING startdate, INT days)`：从开始时间`startdate`增加`days`天
- 返回值是STRING




## day
`day(STRING date)`：提取时间字符串中的天
`dayofmonth(STRING date)`：返回当月第几天
- 返回值是INT

## dayofmonth
`DAYOFMONTH(STRING date)`：返回当月第几天
- 返回值是INT

## decode
`DECODE(BINARY bin, STRING charset)`：使用指定的字符集charset将二进制值bin解码成字符串
- 若任一输入参数为NULL，则结果为NULL
- 返回值是STRING
- 支持的字符集有：
  - US_ASCII
  - ISO-8859-1
  - UTF-8
  - UTF-16BE
  - UTF-16LF
  - UTF-16

## dense_rank
`DENSE_RANK()`：生成数据项在分组中的排名，排名相等的名次一样，序号**不会**在中间留下空白

- [RANK](#rank)：生成数据项在分组中的排名，排名相等的名次一样，序号**会**在中间留下空白

- 其他窗口函数：[ROW_NUMBER](#row-number)、[RANK](#rank)


## encode
`ENCODE(STRING str, STRING charset)`：使用指定的字符集charset将字符串str编码成二进制值
- 若任一输入参数为NULL，则结果为NULL
- 返回值是BINARY
- 支持的字符集有：
  - US_ASCII
  - ISO-8859-1
  - UTF-8
  - UTF-16BE
  - UTF-16LF
  - UTF-16

## find_in_set()
`FIND_IN_SET(STRING s, STRING commaSeparatedString)`：返回在以逗号分隔的字符串commaSeparatedString中s出现的位置
- 返回值是INT
- 如果没找到，则返回NULL

## format_number
`FORMAT_NUMBER(NUMBER x, INT d)`：将数值x转换成“#,###,###.##”格式字符串，并保留d位小数。
- 返回值是STRING
- 如果d为0，则输出整数字符串

## from_unixtime
`FROM_UNIXTIME(BIGINT unixtime[, STRING format])`：将时间戳秒数转换为UTC时间
- 返回值是STRING
- 可以通过format指定输出的时间格式
  - 'yyyyMMdd'
  - 'yyyy-MM-dd'


## from_utc_timestamp
`FROM_UTC_TIMESTAMP(TIMESTAMP timestamp, STRING timezone)`：如果给定的时间戳`timestamp`不是UTC，则将其转换成指定时区`timezone`下的时间戳
- 返回值是TIMESTAMP

## get_json_object
`GET_JSON_OBJECCT(STRING json_string, STRING path)`：从给定路径上的JSON字符串中抽取出JSON对象，并返回这个对象的JSOM字符串形式
- 返回值是STRING
- 如果输入的JSON字符串是非法的，则返回NULL

## hour
`HOUR(STRING date)`：提取时间戳字符串中的小时
- 返回值是INT

## in
`test IN (val1, val2, ...)`：判断test是否等于后面列表中的任一值，是的话返回true
- 返回值是BOOLEAN

## in_file
`IN_FILE(STRING s, STRING filename)`：如果文件名为filename的文件中有完整一行数据和字符串s完全匹配，则返回true
- 返回值是BOOLEAN


## instr
`INSTR(STRING str, STRING substr)`：查找字符串str中子字符串substr第一次出现的位置
- 返回值是INT

## length
`LENGTH(STRING s)`：返回字符串s的长度
- 返回值是INT

## locate
`LOCATE(STRING substr, STRING str[, INT pos])`：查找在字符串str中的pos位置后字符串substr第一次出现的位置
- 返回值是INT

## lower(), lcase()
`LOWER(STRING s)` 或 `LCASE(STRING s)`：将字符串s中所有字母转换成小写字母
- 返回值是STRING
- 与`LCASE()`一样


## lpad
`LPAD(STRING s, INT len, STRING pad)`：从左边开始对字符串s使用字符串pad进行填充，直到达到len长度为止
- 如果s本身长度比len大，则多余的部分会被去除
- 返回值是STRING

## ltrim
`LTRIM(STRING s)` ：将字符串s前面出现的空格全部去除

## minuite
`MINUTE(STRING date)`：提取时间字符串中的分钟数
- 返回值是INT

## month
`MONTH(STRING date)`：提取时间字符串中的月份
- 返回值是INT

```sql
MONTH('2020-09-01 12:00:05')
-- 9

MONTH('2020-09-02')
-- 9
```

## ngrams
`NGRAMS(ARRAY<ARRAY<string>>, INT n, INT k, INT pf)`：估算文件前k个字尾
- 其中pf是精度系数
- 返回值是`ARRAY<STRUCT<STRING, DOUBLE>>`
（不理解）


## ntile()
`NTILE(INT n)`：将分组数据按照顺序切分为n片；返回当前切片值
`NTILE(INT n) OVER(PARTITION BY col_1 ORDER BY col_2)`
- `NTILE()`不支持`ROWS BETWEEN`




## parse_url()
`PARSE_URL(STRING url, STRING partname[, STRING key])`：从url中抽取指定部分的内容
- 参数`partname`：要抽取的部分名称（是大小写敏感的）；可选的值有：
  - HOST
  - PATH
  - QUERY
  - REF
  - PROTOCOL
  - AUTHORITY
  - FILE
  - USERINFO
  - QUERY:<key>
- 如果partname是QUERY的话，则还需要指定第三个参数key
- 返回值是STRING

## printf()
`PRINTF(STRING format, Obj ... args)`：按照printf风格格式化输出输入的字符串
- 返回值是STRING


## rand()
产生随机数
```sql
-- 分桶抽样
SELECT * FROM numbers
TABLESAMPLE(BUCKET 3 OUT OF 10 ON RAND()) s;
-- 分母10表示的是数据将会被散列的桶的个数
-- 分子3表示的是将会选择的桶的个数
```

## rank
`RANK()`：从1开始，按照顺序生成分组内记录的序列
- 序号是连续的，且间隔为1

- 其他窗口函数：[ROW_NUMBER](#row-number)、[DENSE_RANK](#dense-rank)




## regexp_extract
`REGEXP_EXTRACT(STRING s, STRING regex_pattern, STRING index)`：抽取字符串st中符合正则表达式regex_pattern的第index个部分的子字符串
- 返回值是STRING

## regexp_replace
`REGEXP_REPLACE(STRING s, STRING regex, STRING replacement)`：按照Java正则表达式regex将字符串s中符合条件的部分替换成replacement所指定的字符串
- 如果`replacement`为空，则将符合正则表达式的部分去掉
- 返回值是STRING

## repeat
`REPEAT(STRING s, INT n)`：重复输出n次字符串s
- 返回值是STRING

## reverse
`REVERSE(STRING s)`：反转字符串
- 返回值是STRING

## row_number


## rpad
`RPAD(STRING s, INT len, STRING pad)`：从右边开始对字符串s使用字符串pad进行填充，直到达到len长度为止
- 如果字符串s本身长度比len大，则多余的部分被去除
- 返回值是STRING


## rtrim
`RTRIM(STRING s)`：将字符串s后面出现的空格全部去除
- 返回值是STRING

## second
`SECOND(STRING date)`：提取时间字符串中的秒数
- 返回值是INT

```sql
SECOND('2020-09-01 12:23:34')
-- 34
```

## sentences
`SENTENCES(STRING s, STRING lang, STRING locale)`：将输入字符串s转换成句子数组，每个句子又由一个单词数组构成。
- 参数`lang`：可选
- 参数`locale`：可选
- 返回值是`ARRAY<ARRAY<STRING>>`

## size
`SIZE(MAP<key, value>)`：返回MAP中元素的个数
`SIZE(ARRAY<...>)`：返回数组ARRAY的元素个数
- 返回值是INT

## space()
`SPACE(INT n)`：返回n个空格
- 返回值是STRING

## split
`SPLIT(STRING s, STRING pattern)`：按照正则表达式pattern分割字符串s，并将分割后的部分以字符串数组的方式返回
- 返回值是`ARRAY<STRING>`

## str_to_map()
`STRING_TO_MAP(STRING s, STRING delim1, STRING delim2)`：将字符串s按照指定分隔符转换成MAP
- delim1：键值对之间的分隔符（对与对之间）
- delim2：键和值之间的分隔符（对内的键和值之间）
- 返回值是MAP<STRING, STRING>

## substr(), substring()
`SUBSTR(STRING s, INT start_index, INT length)` 或 `SUBSTRING(STRING s, INT start_index, INT length)`：截取字符串s的从start_index位置开始的、长度为length的子字符串
`SUBSTR(BINARY) s, INT start_index, INT length)` 或 `SUBSTRING(STRING s, INT start_index, INT length)`：截取二进制字节值s的从start_index位置开始的、长度为length的子字符串（Hive v0.12.0新增）
- 包括start_index位置上的字符在内
- 返回值是STRING

## tablesample()
数据抽样
1. 分桶抽样
  ```sql
  -- 分桶抽样
  SELECT * FROM numbers
  TABLESAMPLE(BUCKET 3 OUT OF 10 ON RAND()) s;
  -- 分母10表示的是数据将会被散列的桶的个数
  -- 分子3表示的是将会选择的桶的个数

  SELECT * FROM numbers
  TABLESAMPLE(BUCKET 3 OUT OF 10 ON number) s;
  -- 根据number字段分桶
  ```

2. 数据块抽样
  ```sql
  -- 基于行数按照数据块百分比进行抽样
  SELECT * FROM numbersflat
  TABLESAMPLE(0.1 PERCENT) s;
  -- 数据块抽样（百分比抽样）的最小抽样单元是一个HDFS数据块
  -- 如果表的数据大小小于普通的块大小（128MB），则将会返回所有行

  TABLESAMPLE(n M)  -- 指定抽样数据的大小，单位为M

  TABLESAMPLE(n rows)  -- 指定抽样数据的行数
  -- n代表每个map任务均取n行数据
  -- map梳理可以通过hive表的简单查询语句确认（number of mappers: x）
  ```
3. 随机抽样
  ```sql
  SELECT *
  FROM table_name
  WHERE col=xxx 
  DISTRIBUTE BY RAND() 
  SORT BY RAND()
  LIMIT num;
  -- 使用RAND()函数进行随机抽样

  SELECT *
  FROM table_name
  WHERE col=xxx
  ORDER BY RAND()
  LIMIT num;
  -- ORDER BY方式耗时更长
  ```

## to_date()
`TO_DATE(STRING timestamp)`：返回时间字符串的日期部分
- 返回值是STRING
- 时间字符串的格式为“年-月-日”，不能为“年-月”

```sql
TO_DATE('1970-01-01 00:00:00')
-- "1970-01-01"
```

## to_utc_timestamp()
`TO_UTC_TIMESTAMP(TIMESTAMP timestamp, STRING timezone)`：如果给定的时间戳`timestamp`是指定时区`timezone`下的时间戳，则将其转化为UTC下的时间戳
- 返回值是TIMESTAMP

## translate()
？？？

## trim()
`TRIM(STRING s)`：将字符串s前后出现的空格全部去除掉
- 返回值是STRING



## unbase64()
`UNBASE64(STRING str)`：将基于64位的字符串str转换成二进制值
- BASE64字符表：ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
- 返回值是BINARY


## unix_timestamp()
`UNIX_TIMESTAMP()`：获取当前本地时区下的当前时间戳
`UNIX_TIMESTAMP(STRING date)`：输入的时间字符串格式必须是`yyyy-MM-dd HH:mm:ss`。如果不符合，则返回0；如果符合，则将此时间字符串转换成Unix时间戳
`UNIX_TIMESTAMP(STRING date, STRING pattern)`：将指定时间字符串格式字符串转换成Unix时间戳。如果格式不符合，则返回0。
- 返回值是BIGINT



## upper(), ucase()
`UPPER(STRING s)` 或 `UCASE(STRING s)`：将字符串s中所有字母转换成大写字母

## weekofyear()
`WEEKOFYEAR(STRING date)`：返回时间字符出位于一年中的第几周内
- 返回值是INT

```sql
WEEKOFYEAR('2016-12-08 10:05:15')
-- 49
```

## year()
`YEAR(STRING date)`：提取时间字符串中的年份
- 返回值是INT







## 


91页






```sql

```

# 属性配置


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
      <th>属性名称</th>
      <th>缺省值</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>hive.cli.print.header</th>
      <td></td>
      <td>是否让CLI打印出字段名称<br>`=true`：让CLI打印出字段名称<br>`=false`：不打印出字段名称</td>
    </tr>
    <tr>
      <th>hive.exec.dynamic.partition</th>
      <td>false</td>
      <td>`=true`：开启动态分区功能</td>
    </tr>
    <tr>
      <th>hive.exec.dynamic.partition.mode</th>
      <td>strict</td>
      <td>`=nonstrict`：允许所有分区都是动态的</td>
    </tr>
    <tr>
      <th>hive.exec.max.created.files</th>
      <td>100000</td>
      <td>全局可以创建的最大文件个数。<br>有一个Hadoop计数器会跟踪记录已创建的文件个数，若超过该值，则会抛出一个致命错误信息。</td>
    </tr>
    <tr>
      <th>hive.exec.max.dynamic.partitions</th>
      <td>+1000</td>
      <td>一个动态分区创建语句可以创建的最大动态分区个数。<br>若超过该值，则会抛出一个致命错误信息。</td>
    </tr>
    <tr>
      <th>hive.exec.max.dynamic.partitions.pernode</th>
      <td>100</td>
      <td>每个mapper或reducer可以创建的最大动态分区数。<br>如果某个mapper或reducer尝试创建大于这个值的分区，则会抛出一个致命错误信息</td>
    </tr>
    <tr>
      <th>hive.exec.mode.local.auto</th>
      <td>false</td>
      <td>`=true`：Hive会尝试使用本地模式执行其他操作；否则Hive使用MapReduce来执行其他所有的查询</td>
    </tr>
    <tr>
      <th>hive.map.aggr</th>
      <td></td>
      <td>`=true`：触发在map阶段进行的“顶级”聚合过程，提高聚合的性能。<br>非顶级的聚合过程将会在执行一个  `GROUP BY`后进行。</td>
    </tr>
    <tr>
      <th>hive.mapred.mode</th>
      <td></td>
      <td>`=strict`：如果任务对分区表进行查询而WHERE子句没有加分区过滤，则会禁止提交这个任务；会阻止用户执行笛卡尔积查询；执行ORDER BY操作时需要加LIMIT语句进行限制。<br>`=nonstrict`：如果任务对分区表进行查询而WHERE子句没有加分区过滤，这个任务并不会被禁止</td>
    </tr>
    <tr>
      <th></th>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th></th>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th></th>
      <td></td>
      <td></td>
    </tr>
    <tr>
  </tbody>
</table>
</div>


## hive.cli.print.header
`hive.cli.print.header`：是否让CLI打印出字段名称

```sql
-- 让CLI打印出字段名称
SET hive.cli.print.header=true;
```

如果要设置为默认选项，则需将第一行添加到`$HOME/.hiverc`文件中。

## hive.mapred.mode
`hive.mapred.mode=strict`：如果任务对分区表进行查询而WHERE子句没有加分区过滤，则会禁止提交这个任务。
- 防止触发一个巨大的MapReduce任务

```sql
SET hive.mapred.mode=strict;

SELECT 
  t.name
  , t.salary
FROM 
  table_1
LIMIT 10;
```

`hive.mapred.mode=nonstrict`：如果任务对分区表进行查询而WHERE子句没有加分区过滤，这个任务并不会被禁止。


# 视图
- 视图是一个逻辑结构，不会存储数据

**用途**：
1. 降低查询复杂度
  ```sql
  CREATE VIEW shorter_join AS 
  SELECT * FROM people 
  JOIN cart
  ON cart.people_id=people.id
  WHERE first_name = 'John';
  ```
2.限制基于条件过滤的数据，保护信息不被随意查询
  ```sql
  CREATE VIEW temp_view AS 
  SELECT first_name, last_name 
  FROM table_name
  WHERE department='dep_name';
  -- 只提供特定部门的员工信息
  -- 有效保护信息
  ```

- 一个视图的名称要和这个视图所在的数据库下的其他表和视图的名称不同





# 参考资料
- [HIVE编程指南]()
- [Hive实现数据抽样的三种方法](https://blog.csdn.net/zylove2010/article/details/78290319)