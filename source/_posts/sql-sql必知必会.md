---
title: SQL | 《sql必知必会》
date: 2020-05-12 21:27:26
tags: [SQL]
categories: [数据库]
mathjax: true
toc: true
hide: true
notshow: true
index_img: /img/SQL.jpg  ## 封面图片
---

<center>《SQL必知必会》读书笔记</center>
<!--more-->


# 基本
- **数据库**（database）：保存有组织的数据的容器
 - 通常是一个文件或一组文件
- **数据库管理系统**（DBMS，Database Management System）：数据库软件
 - 数据库是通过DBMS创建和操作的容器
- **表**（table）：某种特定类型数据的结构化清单
 - 存储在表中的数据是同一种类型的数据或清单
 - 在相同数据库中不能两次使用相同的表名，在不同的数据库中可以使用相同的表名
 - 表由列组成
- **模式**（schema）：关于数据库和表的布局及特性的信息
- **列**（column）：表中的一个字段。所有表都是由一个或多个列组成的
 - 数据库中的每个列都有相应的数据类型
- **行**（row）：表中的一个记录
 - 表中的数据按行存储
- **数据类型**（datatype）：所允许的数据的类型
 - 每个表列都有相应的数据类型，限制或允许该列中存储的数据
 - 数据类型还帮助正确地分类数据，并在优化磁盘使用方面起重要作用
- **主键**（primary key）：一列（或一组列），其值能够唯一标识表中每一行
 - 主键用来表示一个特定的行
 - 主键应满足：
     - 任意两行都不具有相同的主键值
     - 每一行都必须具有一个主键值（主键列不允许为NULL）
     - 主键列中的值不允许修改或更新
     - 主键值不能重复使用（如果某行从表中删除，它的主键不能赋给新行）
- **子句**（clause）：SQL语句由子句构成；一个子句通常由一个关键字加上所提供的数据组成 

**SQL**（发音为S-Q-L或sequel）：结构化查询语言（Structural Query Language），专门用来与数据库沟通的语言
- SQL语句不区分大小写
 - 多数SQL开发人员喜欢对SQL关键字使用大写，而对列名和表名使用小写，使代码更易于阅读和调试
- 多条SQL语句必须以分号分隔
 - 多数DBMS不需要在单条SQL语句后面加分号
- 在处理SQL语句时，其中所有空格都被忽略
 - 多数SQL开发人员认为，将SQL语句分成多行更容易阅读和调试


## 注释
- 行内注释：使用两个连字符（`--`）或井字符（`#`）
- 多行注释：`/*      */`


# SELECT语句
使用SELECT检索表数据，必须至少给出两条信息：
1. 想选择什么
2. 从什么地方选择

从一个表中检索多列时，列名之间必须以逗号分隔

<u>表：SELECT的子句顺序</u>

|**子句**|**说明**|**是否必须使用**|
|:------:|:-----:|:--------------:|
|SELECT|要选择的列/表达式|是|
|FROM|从中检索数据的表|仅在从表中选择数据时使用|
|WHERE|行级过滤|否|
|GROUP BY|分组说明|仅在按组计算聚合时使用|
|HAVING|组级过滤|否|
|ORDER BY|输出排序顺序|否|

## AS
AS：使用别名
- 可选
- 别名可以是一个单词或一个字符串；是字符串的话，需要放在引号中
- 别名有时也称为导出列（derived column）
- Oracle不支持AS关键字；直接指定列名的别名即可

## DISTINCT关键字
DISTINCT关键字：指示数据库只返回不同的值
- 作用于所有的列，而不仅仅是跟在它之后的那一列

```sql
mysql> SELECT vend_id FROM Products;
+---------+
| vend_id |
+---------+
|    1001 |
|    1001 |
|    1001 |
|    1002 |
|    1002 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1005 |
|    1005 |
+---------+
14 rows in set (0.01 sec)
mysql> SELECT DISTINCT vend_id FROM Products;
+---------+
| vend_id |
+---------+
|    1001 |
|    1002 |
|    1003 |
|    1005 |
+---------+
4 rows in set (0.01 sec)
```

## 限制返回的行数
- SQL Server或Acess：`TOP`
 ```sql
 -- 返回前5行数据
 SELECT TOP 5 prod_name FROM Products;
 ```
- DB2：`FETCH FIRST ... ROWS ONLY`
 ```sql
 SELECT TOP 5 prod_name FROM Products;
 SELECT prod_name FROM Products 
 FETCH FIRST 5 ROWS ONLY;
 ```
- Oracle：行计数器 `ROWNUM`
 ```sql
 SELECT prod_name FROM Products
 WHERE ROWNUM<=5;
 ```
- MySQL、MariaDB、PostgreSQL、SQLite：`LIMIT`
 ```sql
 mysql> SELECT prod_name FROM Products LIMIT 5;
 +--------------+
 | prod_name    |
 +--------------+
 | .5 ton anvil |
 | 1 ton anvil  |
 | 2 ton anvil  |
 | Detonator    |
 | Bird seed    |
 +--------------+
 5 rows in set (0.00 sec)
 ```

## 限制检索的起始位置
限制检索的起始位置：`OFFSET`
 ```sql
 -- 从第3行起（不包括第3行）返回前5行数据
 mysql> SELECT prod_name FROM Products LIMIT 5 OFFSET 3;
 +--------------+
 | prod_name    |
 +--------------+
 | Detonator    |
 | Bird seed    |
 | Carrots      |
 | Fuses        |
 | JetPack 1000 |
 +--------------+
 5 rows in set (0.00 sec)
 ```

- 第一个被检索的行是第0行，而不是第一行
> `LIMIT 3 OFFSET 1` 会检索第2行，而不是第1行

- MySQL和MariaDB会支持简化版的LIMIT OFFSET 语句，但是两个数字的顺序改变了
> `LIMIT 5 OFFSET 3` 等价于 `LIMIT 3, 5`

```sql
mysql> SELECT prod_name FROM Products LIMIT 3, 5;
+--------------+
| prod_name    |
+--------------+
| Detonator    |
| Bird seed    |
| Carrots      |
| Fuses        |
| JetPack 1000 |
+--------------+
5 rows in set (0.00 sec)
```

# ORDER BY子句
排序：ORDER BY 子句
- 必须是SELECT语句中的最后一个子句
- 默认升序排列（ASC）；如果要降序排列，需指定DESC
- DESC关键字只应用到直接位于其前面的列名
- 如果对多个列进行降序排列，则必须对每一列指定DESC关键字
- 大多数DBMS认为A与a相同；数据管理员可以改变这种行为
- 按多个列排序的话，列名之间用逗号分开；先排序的在前面，后排序的在后面
- Microsoft Access不允许按别名排序
```sql
mysql> SELECT prod_id, prod_price, prod_name
    -> FROM Products;
+---------+------------+----------------+
| prod_id | prod_price | prod_name      |
+---------+------------+----------------+
| ANV01   |       5.99 | .5 ton anvil   |
| ANV02   |       9.99 | 1 ton anvil    |
| ANV03   |      14.99 | 2 ton anvil    |
| DTNTR   |      13.00 | Detonator      |
| FB      |      10.00 | Bird seed      |
| FC      |       2.50 | Carrots        |
| FU1     |       3.42 | Fuses          |
| JP1000  |      35.00 | JetPack 1000   |
| JP2000  |      55.00 | JetPack 2000   |
| OL1     |       8.99 | Oil can        |
| SAFE    |      50.00 | Safe           |
| SLING   |       4.49 | Sling          |
| TNT1    |       2.50 | TNT (1 stick)  |
| TNT2    |      10.00 | TNT (5 sticks) |
+---------+------------+----------------+
14 rows in set (0.00 sec)
mysql> SELECT prod_id, prod_price, prod_name
    -> FROM Products
    -> ORDER BY prod_price, prod_name;
+---------+------------+----------------+
| prod_id | prod_price | prod_name      |
+---------+------------+----------------+
| FC      |       2.50 | Carrots        |
| TNT1    |       2.50 | TNT (1 stick)  |
| FU1     |       3.42 | Fuses          |
| SLING   |       4.49 | Sling          |
| ANV01   |       5.99 | .5 ton anvil   |
| OL1     |       8.99 | Oil can        |
| ANV02   |       9.99 | 1 ton anvil    |
| FB      |      10.00 | Bird seed      |
| TNT2    |      10.00 | TNT (5 sticks) |
| DTNTR   |      13.00 | Detonator      |
| ANV03   |      14.99 | 2 ton anvil    |
| JP1000  |      35.00 | JetPack 1000   |
| SAFE    |      50.00 | Safe           |
| JP2000  |      55.00 | JetPack 2000   |
+---------+------------+----------------+
14 rows in set (0.01 sec)
```
- 还支持按照SELECT清单中的列的相对位置进行排序
```sql
mysql> SELECT prod_id, prod_price, prod_name
    -> FROM Products
    -> ORDER BY 2, 3;
+---------+------------+----------------+
| prod_id | prod_price | prod_name      |
+---------+------------+----------------+
| FC      |       2.50 | Carrots        |
| TNT1    |       2.50 | TNT (1 stick)  |
| FU1     |       3.42 | Fuses          |
| SLING   |       4.49 | Sling          |
| ANV01   |       5.99 | .5 ton anvil   |
| OL1     |       8.99 | Oil can        |
| ANV02   |       9.99 | 1 ton anvil    |
| FB      |      10.00 | Bird seed      |
| TNT2    |      10.00 | TNT (5 sticks) |
| DTNTR   |      13.00 | Detonator      |
| ANV03   |      14.99 | 2 ton anvil    |
| JP1000  |      35.00 | JetPack 1000   |
| SAFE    |      50.00 | Safe           |
| JP2000  |      55.00 | JetPack 2000   |
+---------+------------+----------------+
14 rows in set (0.00 sec)
```

# WHERE子句
**过滤数据**：指定搜索条件（search criteria）/过滤条件（filter condition），只检索所需数据
- 在SELECT语句中，数据根据WHERE子句中指定的搜索条件进行过滤
- WHERE子句在表名（FROM子句）之后给出
```sql
mysql> SELECT prod_name, prod_price
    -> FROM Products
    -> WHERE prod_price=2.5;
+---------------+------------+
| prod_name     | prod_price |
+---------------+------------+
| Carrots       |       2.50 |
| TNT (1 stick) |       2.50 |
+---------------+------------+
2 rows in set (0.00 sec)
```
- 同时使用ORDER BY子句和WHERE子句时，ORDER BY 应位于WHERE子句后面

<u>表：WHERE子句操作符</u>

|**操作符**|**说明**|
|:---:|:----:|
|=|等于|
|<>  或  !=	|不等于|
|<|小于|
|<=  或  !>|小于等于 或 不大于|
|>|大于|
|>= 或 !<|大于等于 或 不小于|
|BETWEEN|在指定的两个值之间|
|IS NULL|为NULL值|

- `!=` 和 `<>` 通常是可以互换的。但并非所有DBMS都支持这两种操作
> eg：Microsoft Access支持 `<>`而不支持 `!=`

## BETWEEN
`BETWEEN`匹配范围中的所有值，包括指定的开始值和结束值
```sql
mysql> ##查找价格在5到10之间的产品
mysql> SELECT prod_name, prod_price
    -> FROM Products
    -> WHERE prod_price BETWEEN 5 AND 10;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| .5 ton anvil   |       5.99 |
| 1 ton anvil    |       9.99 |
| Bird seed      |      10.00 |
| Oil can        |       8.99 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
5 rows in set (0.01 sec)
```
## NULL
**NULL**：无值/空值（no value），与字段包含0、空字符串、空格不同
- 用`IS NULL`来检查某个值是否为NULL
```sql
mysql> SELECT cust_name
    -> FROM CUSTOMERS
    -> WHERE cust_email IS NULL;
+-------------+
| cust_name   |
+-------------+
| Mouse House |
| E Fudd      |
+-------------+
2 rows in set (0.01 sec)
```

## 操作符
操作符（operator）：用来联结或改变WHERE子句中的子句的关键字，也称逻辑操作符（logical operator）
- `AND`、`OR`、`IN`、`NOT`
- SQL允许给出多个WHERE子句，使用AND或OR联结

### AND
AND：用在WHERE子句中的关键字，用来指示检索满足所有给定条件的行
```sql
mysql> SELECT prod_id, prod_price, prod_name
    -> FROM Products
    -> WHERE vend_id=1003 AND prod_price <= 4;
+---------+------------+---------------+
| prod_id | prod_price | prod_name     |
+---------+------------+---------------+
| FC      |       2.50 | Carrots       |
| TNT1    |       2.50 | TNT (1 stick) |
+---------+------------+---------------+
2 rows in set (0.00 sec)
```

### OR
OR：用在WHERE子句中的关键字，用来表示检索匹配任一给定条件的行
- AND的优先级高于OR
- 同时使用AND和OR时，注意使用圆括号对操作符进行明确分组
```sql
mysql> SELECT prod_id, prod_price, prod_name
    -> FROM Products
    -> WHERE vend_id=1003 OR vend_id=1002;
+---------+------------+----------------+
| prod_id | prod_price | prod_name      |
+---------+------------+----------------+
| DTNTR   |      13.00 | Detonator      |
| FB      |      10.00 | Bird seed      |
| FC      |       2.50 | Carrots        |
| FU1     |       3.42 | Fuses          |
| OL1     |       8.99 | Oil can        |
| SAFE    |      50.00 | Safe           |
| SLING   |       4.49 | Sling          |
| TNT1    |       2.50 | TNT (1 stick)  |
| TNT2    |      10.00 | TNT (5 sticks) |
+---------+------------+----------------+
9 rows in set (0.00 sec)
```

### IN
IN：WHERE子句中用来指定要匹配值的清单的关键字，功能与OR相当
- IN操作符一般比一组OR操作符执行得更快
```sql
mysql> SELECT prod_id, prod_price, prod_name
    -> FROM Products
    -> WHERE vend_id IN (1002, 1003);
+---------+------------+----------------+
| prod_id | prod_price | prod_name      |
+---------+------------+----------------+
| DTNTR   |      13.00 | Detonator      |
| FB      |      10.00 | Bird seed      |
| FC      |       2.50 | Carrots        |
| FU1     |       3.42 | Fuses          |
| OL1     |       8.99 | Oil can        |
| SAFE    |      50.00 | Safe           |
| SLING   |       4.49 | Sling          |
| TNT1    |       2.50 | TNT (1 stick)  |
| TNT2    |      10.00 | TNT (5 sticks) |
+---------+------------+----------------+
9 rows in set (0.00 sec)
```

### NOT
NOT：WHERE子句中用来否定其后条件的关键字
- 常与IN联合使用
- 大多数DBMS允许使用`NOT`否定任何条件
- MariaDB支持使用`NOT`否定IN、BETWEEN、EXISTS子句
```sql
mysql> SELECT prod_id, prod_price, prod_name
    -> FROM Products
    -> WHERE NOT vend_id=1003
    -> ORDER BY prod_name;
+---------+------------+--------------+
| prod_id | prod_price | prod_name    |
+---------+------------+--------------+
| ANV01   |       5.99 | .5 ton anvil |
| ANV02   |       9.99 | 1 ton anvil  |
| ANV03   |      14.99 | 2 ton anvil  |
| FU1     |       3.42 | Fuses        |
| JP1000  |      35.00 | JetPack 1000 |
| JP2000  |      55.00 | JetPack 2000 |
| OL1     |       8.99 | Oil can      |
+---------+------------+--------------+
7 rows in set (0.00 sec)
```

# pattern
搜索模式（search pattern）：由字面值、通配符或两者组合构成的搜索条件

谓词（predicate）：操作符作为谓词时不是操作符
- 从技术上来说，LIKE是谓词而不是操作符

## 通配符
通配符（wildcard）：用来匹配值得一部分的特殊字符
- 通配符实质上是WHERE子句中有特殊意义的字符
- 要在WHERE子句中使用通配符，必须使用LIKE操作符
- 通配符搜索只能用于文本字段（字符串），非文本数据类型的字段不能使用通配符搜索
- 不要过度使用通配符
- 注意通配符的位置

### %
`%` ：百分号通配符，表示任何字符出现任意次数
- Microsoft Access使用的通配符是 `*` 而不是 `%`
- 根据DBMS的不同及其配置，搜索可以是区分大小写的
- 通配符可在搜索模式的任意位置使用，并且可以使用多个通配符
- `%` 代表搜索模式中给定位置的0个、1个或多个字符
- `%` 不会匹配NULL
```sql
mysql> ##找出prod_name以Jet开头的产品
mysql> SELECT prod_id, prod_name
    -> FROM Products
    -> WHERE prod_name LIKE 'Jet%';
+---------+--------------+
| prod_id | prod_name    |
+---------+--------------+
| JP1000  | JetPack 1000 |
| JP2000  | JetPack 2000 |
+---------+--------------+
2 rows in set (0.00 sec)
```

### _
`_` ：下划线通配符；用途与 `%` 一样，但是仅匹配单个字符
- DB2不支持下划线通配符
- Microsoft Access中对应的是 `？`而不是 `_`

### []
`[ ]` ：方括号通配符，用来指定一个字符集；必须匹配指定位置的一个字符
- 只有Microsoft Access和SQL Server支持集合
- 使用 `^` 来表示否定；Microsoft Access是用 `！`表示否定而不是 `^`
```sql
## 找出姓名首字母为V或T的学生信息
SELECT ID, name, dept_name
FROM student
WHERE name LIKE '[VT]%';
## 找出姓名首字母不为V且不为T的学生信息
SELECT ID, name, dept_name
FROM student
WHERE name LIKE '[^VT]%';
```

# 计算字段
字段（field）：基本上与列（column）的意思相同，经常互换使用；数据库列一般称为列，而字段通常与计算字段一起使用

## 拼接
**拼接**（concatenate）：将值联结到一起（将一个值附加到另一个值）构成单个值
- Access、SQL Server使用 `+` 拼接
- DB2、Oracle、PostgreSQL、SQLite、Open Office Base使用 `||` 拼接
- MySQL、MariaDB使用特殊函数 `CONCAT()`

```sql
mysql> SELECT * FROM Vendors;
+---------+----------------+-----------------+-------------+------------+----------+--------------+
| vend_id | vend_name      | vend_address    | vend_city   | vend_state | vend_zip | vend_country |
+---------+----------------+-----------------+-------------+------------+----------+--------------+
|    1001 | Anvils R Us    | 123 Main Street | Southfield  | MI         | 48075    | USA          |
|    1002 | LT Supplies    | 500 Park Street | Anytown     | OH         | 44333    | USA          |
|    1003 | ACME           | 555 High Street | Los Angeles | CA         | 90046    | USA          |
|    1004 | Furball Inc.   | 1000 5th Avenue | New York    | NY         | 11111    | USA          |
|    1005 | Jet Set        | 42 Galaxy Road  | London      | NULL       | N16 6PS  | England      |
|    1006 | Jouets Et Ours | 1 Rue Amusement | Paris       | NULL       | 45678    | France       |
+---------+----------------+-----------------+-------------+------------+----------+--------------+
6 rows in set (0.00 sec)

mysql> SELECT CONCAT(vend_name, '(', vend_country, ')')
    -> FROM Vendors
    -> ORDER BY vend_name;
+-------------------------------------------+
| CONCAT(vend_name, '(', vend_country, ')') |
+-------------------------------------------+
| ACME(USA)                                 |
| Anvils R Us(USA)                          |
| Furball Inc.(USA)                         |
| Jet Set(England)                          |
| Jouets Et Ours(France)                    |
| LT Supplies(USA)                          |
+-------------------------------------------+
6 rows in set (0.01 sec)
```

SQL Server：
```sql
SELECT ID + '(' + dept_name + ')'
FROM student
ORDER BY ID;
```

<u>表 ：SQL算术操作符</u>

|操作符|说明|
|:--:|:---:|
|+|加|
|-|减|
|*|乘|
|/|除|

# 字符串函数
可移植（portable）：所编写的代码可以在多个系统上运行
- SQL函数不可移植

## SUBSTR()
提取字符串的组成部分：
- Access：使用 `MID()`
- DB2、Oracle、PostgreSQL、SQLite：使用 `SUBSTR()`
- MySQL、SQL Server：使用 `SUBSTRING()`

```sql
mysql> SELECT * FROM Vendors;
+---------+----------------+-----------------+-------------+------------+----------+--------------+
| vend_id | vend_name      | vend_address    | vend_city   | vend_state | vend_zip | vend_country |
+---------+----------------+-----------------+-------------+------------+----------+--------------+
|    1001 | Anvils R Us    | 123 Main Street | Southfield  | MI         | 48075    | USA          |
|    1002 | LT Supplies    | 500 Park Street | Anytown     | OH         | 44333    | USA          |
|    1003 | ACME           | 555 High Street | Los Angeles | CA         | 90046    | USA          |
|    1004 | Furball Inc.   | 1000 5th Avenue | New York    | NY         | 11111    | USA          |
|    1005 | Jet Set        | 42 Galaxy Road  | London      | NULL       | N16 6PS  | England      |
|    1006 | Jouets Et Ours | 1 Rue Amusement | Paris       | NULL       | 45678    | France       |
+---------+----------------+-----------------+-------------+------------+----------+--------------+
6 rows in set (0.00 sec)
mysql> ## 截取vend_address的第4个字符（包括空格）开始(包括第4个字符)的所有字符
mysql> SELECT SUBSTRING(vend_address, 4)
    -> FROM Vendors;
+----------------------------+
| SUBSTRING(vend_address, 4) |
+----------------------------+
|  Main Street               |
|  Park Street               |
|  High Street               |
| 0 5th Avenue               |
| Galaxy Road                |
| ue Amusement               |
+----------------------------+
6 rows in set (0.00 sec)

mysql> ## 截取vend_address的倒数第4个字符开始的所有字符
mysql> SELECT SUBSTRING(vend_address, -4)
    -> FROM Vendors;
+-----------------------------+
| SUBSTRING(vend_address, -4) |
+-----------------------------+
| reet                        |
| reet                        |
| reet                        |
| enue                        |
| Road                        |
| ment                        |
+-----------------------------+
6 rows in set (0.00 sec)
mysql> ## 截取vend_address的第4-6个字符
mysql> SELECT SUBSTRING(vend_address, 4, 3)
    -> FROM Vendors;
+-------------------------------+
| SUBSTRING(vend_address, 4, 3) |
+-------------------------------+
|  Ma                           |
|  Pa                           |
|  Hi                           |
| 0 5                           |
| Gal                           |
| ue                            |
+-------------------------------+
6 rows in set (0.00 sec)
-- 第4~6个字符，共有3个字符
mysql> ## 截取vend_address的倒数第4个~倒数第2个字符
mysql> SELECT SUBSTRING(vend_address, -4, 3)
    -> FROM Vendors;
+--------------------------------+
| SUBSTRING(vend_address, -4, 3) |
+--------------------------------+
| ree                            |
| ree                            |
| ree                            |
| enu                            |
| Roa                            |
| men                            |
+--------------------------------+
6 rows in set (0.00 sec)
```

## TRIM()
TRIM() ：去掉字符串左右两边的空格


## RTRIM()
RTRIM() ：去掉字符串右边的所有空格

## LTRIM()
LTRIM() ：去掉字符串左边的所有空格

## LEFT()
LEFT() ：返回字符串左边的字符

## RIGHT()
RIGHT() ：返回字符串右边的字符

## LENGTH()
LENGTH() ：返回字符串的长度；或`DATALENGTH()` 、`LEN()`

## SOUNDEX()
SOUNDEX() ：返回字符串的SOUNDEX值
- SOUNDEX是一个将任何文本字符串转换为描述其语音表示的字母数字模式的算法
- Microsoft Access、PostgreSQL不支持SOUNDEX()

```sql
mysql> ## 找出cust_contact中发音与Michael Green类似的记录
mysql> SELECT cust_name, cust_contact
    -> FROM Customers
    -> WHERE SOUNDEX(cust_contact)=SOUNDEX('Michael Green');
```

# 日期处理函数
DATEPART() ：返回日期的一部分
- Oracle没有`DATEPART()`函数，可借助 `TO_CHAR()` 和 `TO_NUMBER()`
 - `TO_CHAR()`用来提取日期的成分
 - `TO_NUMBER()`用来将提取出来的成分转换为数值；或用 `BETWEEN()`

```sql
## 检索2012年的所有订单
## SQL Server
SELECT order_num FROM Orders
WHERE DATEPART(yy, order_date) = 2012;

## Access
SELECT order_num FROM Orders
WHERE DATEPART('yyyy', order_date) = 2012;

## PostgreSQL
SELECT order_num FROM Orders
WHERE DATE_PART('year', order_date) = 2012;

## Oracle
SELECT order_num FROM Orders
WHERE TO_NUMBER(TO_CHAR(order_date, 'YYYY')) = 2012;
# 或
SELECT order_num FROM Orders
WHERE order_date BETWEEN TO_DATE('01-01-2012')
AND TO_DATE('12-31-2012');

## MySQL 或 MariaDB
SELECT order_num FROM Orders
WHERE YEAR(order_date) = 2012;

## SQLite
SELECT order_num FROM Orders
WHERE STRFTIME('%Y', order_date) = '2012';
```

# 数值处理函数

<u>表 ：常用数值处理函数</u>

|操作符|说明|
|:---:|:----:|
|ABS()|绝对值|
|COS()|余弦值|
|EXP()|指数值|
|PI()|圆周率Π|
|SIN()|正弦值|
|SQRT()|平方根|
|TAN()|正切|

# 聚合函数
聚合函数（aggregate function）：对某些行运行的函数，计算并返回一个值

<u>表 ：SQL聚合函数</u>

|操作符|说明|
|:---:|:---:|
|AVG()|平均值|
|COUNT()|行数|
|MAX()|最大值|
|MIN()|最小值|
|SUM()|求和|

## AVG()
AVG() ：平均值
- AVG() 只能用来确定特定数值列的平均值，列名必须作为函数参数给出
- AVG() 函数忽略列值为NULL的行

## COUNT()
COUNT() ：计数
- COUNT(*) 对表中行的数目进行计数，不管表列中包含的是空值还是非空值
- COUNT(column) 对特定列中的具有值的行进行计数，忽略NULL值

## MAX()
MAX() ：最大值
- 一般用来找出最大的数值或日期值
- 大多数DBMS允许它用来返回任意列中的最大值，包括返回文本列中的最大值
- 在用于文本数据时，返回按该列排序后的最后一行
- 忽略列值为NULL的行

## MIN()
MIN() ：最小值
- 一般用来找出最小的数值或日期值
- 大多数DBMS允许它用来返回任意列中的最小值，包括返回文本列中的最小值
- 在用于文本数据时，返回按该列排序后的最前面的一行
- 略列值为NULL的行

## SUM()
SUM() ：求和
- 忽略值为NULL的行

## DISTINCT
DISTINCT：
- 以上5个聚合函数，默认对所有的行执行计算，指定ALL参数或补指定（ALL是默认行为）
- 若只考虑对不同的值的行执行计算，则应指定参数 DISTINCT
- Microsoft Access在聚合函数中不支持DISTINCT
- DISTINCT不能用于 COUNT(*)
- DISTINCT必须使用列名，不能用于计算或表达式

```sql
mysql> SELECT AVG(DISTINCT prod_price) AS avg_price
    -> FROM Products
    -> WHERE vend_id=1003;
+-----------+
| avg_price |
+-----------+
| 15.998000 |
+-----------+
1 row in set (0.01 sec)
```

```sql

```

```sql

```

```sql

```

```sql

```

# 参考资料
[1] [SQL必知必会](https://book.douban.com/subject/24250054/)
[2] [本文sql脚本参照](https://forta.com/wp-content/uploads/books/0672327120/mysql_scripts.zip)
[3] [Notes | SQL必知必会 | 01](https://mp.weixin.qq.com/s?__biz=MzU5NzkxODMxOA==&mid=2247486117&idx=1&sn=47aa300d46873749282b368db799afd7&chksm=fe4d5da4c93ad4b2a125503826e381610b79c3905feb0b194907d46bf771dc66b784325ddb30&token=1415910284&lang=zh_CN#rd)