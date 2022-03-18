---
title: HIVE学习笔记
date: 2020-06-22 12:57:53
tags: [HIVE,数据库]
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

## 配置项
### hive.cli.print.header
`hive.cli.print.header`：是否让CLI打印出字段名称

```sql
-- 让CLI打印出字段名称
SET hive.cli.print.header=true;
```

如果要设置为默认选项，则需将第一行添加到`$HOME/.hiverc`文件中。

### hive.mapred.mode
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



## 集合数据类型
### ARRAY
一组具有相同类型和名称的变量的集合。
- 元素：这些变量称为数组的元素
- 每个数组元素都有一个编号，编号从0开始

```json
Array('John', 'Doe')
```
其中`'Doe'`的应用应为`数组名[1]`。

### MAP
- MAP是一组键值对元组集合，使用数组表示法访问元素

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

## 字段分隔符


70页


## 数据库
- 如果没有显式指定数据库，则会使用默认的数据库default



## 表
HIVE中的表分为两种：
1. 管理表，也称内部表
2. 外部表

- 用户可在`DESCRIBE EXTENDED table_name`语句的输出中查看到该表的类型（管理表MANAGED_TABLE，外部表EXTERNAL_TABLE）

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


## get_json_object()

## location
创建数据库时修改数据库的存储位置

```sql
CREATE DATABASE db_name1
LOCATION '/my/preferred/directory';
```


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

## use
用于将某个数据库设置为用户当前的工作数据库

```sql
USE db_name_2;
```


```sql

```



# 参考资料
- [HIVE编程指南]()
- []()