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

### hiveconf配置项
#### hive.cli.print.header
`hive.cli.print.header`：是否让CLI打印出字段名称

```sql
-- 让CLI打印出字段名称
SET hive.cli.print.header=true;
```

如果要设置为默认选项，则需将第一行添加到`$HOME/.hiverc`文件中。


#### hive.cli.print.current.db
`hive.cli.print.current.db`：为true时，在提示符里显示当前所在的数据库（HIVE version$\geq$0.8.0）

```shell
hive> SET hive.cli.print.current.db=true;
hive(db_name_1)> USE default;
hive(default)> SET hive.cli.print.current.db=false;
hive> ...
```



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
      <td>INT</td>
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

```js
{
  'store'
}
```







# 语法
# 基本
## -S
开启静默模式，可将输出结果中的`OK`和`Time taken`等行以及其他一些无关紧要的输出信息

## set
- 可用于给变量赋新的值


## get_json_object()



# 参考资料
- [HIVE编程指南]()
- []()