---
title: MySQL
date: 2020-05-22 20:39:35
tags: [SQL,MySQL]
categories: 数据库
mathjax: true
toc: true
---

<center></center>
<!--more-->

# 存储引擎
MySQL在v5.1之前的默认存储引擎使MyISAM，在此之后的默认存储引擎是InnoDB



# 表的类型
1. **永久表**
 - 创建以后用于长期保存数据的表
2. **临时表**
 - 只保存临时数据
 - 能够长久存在
3. **虚表/视图**
## 永久表

## 临时表
### 内存临时表
- 使用的是MEMORY存储引擎

### 磁盘临时表

## 视图

# 索引
索引是存储在一张表中特定列上的数据结构
- 索引是在列上创建的
- 索引是一种数据结构

## 全局索引
FULLTEXT
- 目前只有MyISAM引擎支持全局索引

## 哈希索引
HASH
- 是MySQL中用到的唯一键值对（key-value）的数据结构
- 哈希索引适合应用于查找单个键的情况
- 在范围查找中，哈希索引的性能很低

## B-Tree索引
平衡树索引
- 存在很多变种
 - B+Tree
 - ……

## R-Tree索引
- 在MySQL中较少使用
- 仅支持geometry数据类型
- 支持该类型的存储引擎有：
 - MyISAM
 - BDb
 - InnoDb
 - NDb
 - Archive
- 相对于B-Tree来说，R-Tree的优势在于范围查找

# MySQL自带数据库
MySQL自带三个数据库：
- information_schema
- performance_schema
- mysql

## information_schema
提供访问数据库元数据的方式；可称information_schema是一个元数据库。

**元数据**（meta data）：关于数据的数据（data about data）。也可称为“数据词典”、“系统目录”
- 数据库名
- 表名
- 列的数据类型
- 访问权限
- ……

**常见的表**：

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
      <th>表</th>
      <th>表的含义</th>
      <th>表中字段</th>
      <th>字段含义</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3">schemata</th>
      <td rowspan="3">提供数据库信息<br>相关命令：SHOW DATABASES;</td>
      <td>schema_name</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>default_character_set_name</td>
      <td>字符集</td>
    </tr>
    <tr>
      <td>default_collation_name</td>
      <td>排序规则</td>
    </tr>
    <tr>
      <th rowspan="5">tables</th>
      <td rowspan="5">提供表的信息<br>相关命令：SHOW TABLES FROM schema_name;</td>
      <td>table_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>table_name</td>
      <td>表名</td>
    </tr>
    <tr>
      <td>table_type</td>
      <td>表的类型<br>base table, view, system view</td>
    </tr>
    <tr>
      <td>engine</td>
      <td>存储引擎</td>
    </tr>
    <tr>
      <td>create_time</td>
      <td>建表时间</td>
    </tr>
    <tr>
      <th rowspan="4">columns</th>
      <td rowspan="4">提供表中字段信息<br>相关命令：SHOW COLUMNS; 或 DESC TCTEST.EMP</td>
      <td>schema_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>table_name</td>
      <td>表名</td>
    </tr>
    <tr>
      <td>column_name</td>
      <td>字段名</td>
    </tr>
    <tr>
      <td>column_type</td>
      <td>字段类型</td>
    </tr>
    <tr>
      <th rowspan="6">statistics</th>
      <td rowspan="6">索引信息<br>相关命令：SHOW INDEX; </td>
      <td>schema_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>table_name</td>
      <td>表名</td>
    </tr>
    <tr>
      <td>index_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>index_name</td>
      <td>索引名</td>
    </tr>
    <tr>
      <td>column_name</td>
      <td>字段名</td>
    </tr>
    <tr>
      <td>index_type</td>
      <td>索引类型<br>一般是BTREE</td>
    </tr>
    <tr>
      <th rowspan="5">table_constraints</th>
      <td rowspan="5">约束信息</td>
      <td>constraint_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>constraint_name</td>
      <td>约束名</td>
    </tr>
    <tr>
      <td>table_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>table_name</td>
      <td>表名</td>
    </tr>
    <tr>
      <td>constraint_type</td>
      <td>约束类型<br>UNIQUE, PRIMARY KEY, FOREIGN KEY</td>
    </tr>
    <tr>
      <th rowspan="8">key_column_usage</th>
      <td rowspan="8">外键的参考信息</td>
      <td>constraint_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>constraint_name</td>
      <td>约束名</td>
    </tr>
    <tr>
      <td>table_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>table_name</td>
      <td>表名</td>
    </tr>
    <tr>
      <td>column_name</td>
      <td>字段名</td>
    </tr>
    <tr>
      <td>referenced_table_schema</td>
      <td>参考的数据库</td>
    </tr>
    <tr>
      <td>referenced_table_name</td>
      <td>参考的表</td>
    </tr>
    <tr>
      <td>referenced_column_name</td>
      <td>参考的列</td>
    </tr>
    <tr>
      <th rowspan="6">routines</th>
      <td rowspan="6">函数和存储过程</td>
      <td>specific_name</td>
      <td>程序名</td>
    </tr>
    <tr>
      <td>routine_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>routine_name</td>
      <td>程序名</td>
    </tr>
    <tr>
      <td>routine_type</td>
      <td>程序类型<br>PROCEDURE或FUNCTION</td>
    </tr>
    <tr>
      <td>routine_body</td>
      <td>函数体</td>
    </tr>
    <tr>
      <td>routine_definition</td>
      <td>具体的程序语句</td>
    </tr>
    <tr>
      <th rowspan="3">views</th>
      <td rowspan="3">视图</td>
      <td>table_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>table_name</td>
      <td>表名</td>
    </tr>
    <tr>
      <td>view_definition</td>
      <td>视图定义语句</td>
    </tr>
    <tr>
      <th rowspan="6">triggers</th>
      <td rowspan="6">触发器<br>相关命令：SHOW TRIGGERS FROM table_name;</td>
      <td>trigger_schema</td>
      <td>数据库名</td>
    </tr>
    <tr>
      <td>trigger_name</td>
      <td>触发器名</td>
    </tr>
    <tr>
      <td>event_object_schema</td>
      <td>触发的数据库</td>
    </tr>
    <tr>
      <td>event_object_table</td>
      <td>触发的表</td>
    </tr>
    <tr>
      <td>action_statement</td>
      <td>触发的语句</td>
    </tr>
    <tr>
      <td>action_timing</td>
      <td>触发的时机<br>BEFORE或AFTER</td>
    </tr>
    <tr>
      <th>engines</th>
      <td>当前数据库对InnoDB、Memory、MyISAM等存储引擎的支持情况<br>相关命令：SHOW ENGINES;</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>global_variables</th>
      <td>服务器变量设置，一些开关和设置<br>相关命令：SHOW GLOBAL VARIABLES;</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>plugins</th>
      <td>插件列表<br>相关命令：SHOW PLUGINS;</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>processlist</th>
      <td>正在运行的线程<br>相关命令：SHOW FULL PROCESSLIST;</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

```sql
SELECT REPEAT('* ', @NUMBER := @NUMBER - 1)
FROM information_schema.tables, 
(SELECT @NUMBER:= 13) t LIMIT 12; 
-- 输出：
* * * * * * * * * * * * 
* * * * * * * * * * * 
* * * * * * * * * * 
* * * * * * * * * 
* * * * * * * * 
* * * * * * * 
* * * * * * 
* * * * * 
* * * * 
* * * 
* * 
* 
-- from: https://www.hackerrank.com/challenges/draw-the-triangle-1/problem?h_r=profile
```

```sql
-- 输出1000以内的所有素数
SELECT GROUP_CONCAT(NUMB SEPARATOR '&')
FROM (
    SELECT @nun:=@num+1 AS NUMB FROM
    information_schema.tables t1,
    information_schema.tables t2,
    (SELECT @num:=1) tmp
) tempNum  -- 所有的可能素数
WHERE NUMB <= 1000 
AND NOT EXISTS (
    SELECT * FROM 
    (
        SELECT @nu:=@nu+1 AS NUMA FROM 
        information_schema.tables t1,
        information_schema.tables t2,
        (SELECT @nu:=1) tmp1
        LIMIT 1000
    ) t  -- NUMA是divisor
    WHERE FLOOR(NUMB/NUMA) = (NUMB/NUMA)
    AND NUMA < NUMB 
    AND NUMA > 1
);

-- 或
SET @potential_prime = 1;
SET @divisor = 1;

SELECT GROUP_CONCAT(POTENTIAL_PRIME SEPARATOR '&') FROM
(
    SELECT @potential_prime := @potential_prime + 1 AS POTENTIAL_PRIME 
    FROM 
        information_schema.tables t1,
        information_schema.tables t2
    LIMIT 1000
) list_of_potential_primes
WHERE NOT EXISTS(
    SELECT * FROM
    (
        SELECT @divisor := @divisor + 1 AS DIVISOR 
        FROM 
            information_schema.tables t3,
            information_schema.tables t4
        LIMIT 1000
    ) list_of_divisors
    WHERE MOD(POTENTIAL_PRIME, DIVISOR) = 0
    AND POTENTIAL_PRIME <> DIVISOR
    );
-- 参考：https://www.hackerrank.com/challenges/print-prime-numbers/forum
```

# 触发器
允许用户定义一组操作，这些操作通过对指定的表进行删除、更新等命令来执行或激活


# 时间
- 时间不能写为`<='2020-07-12'`的形式
 - `<'2020-07-12'`是指2020年07月12日之前，不包括2020年07月12日
 - `<='2020-07-12'`包含了2020年07月12日00时00分00秒，但不包含2020年07月12日一整天
 - 如果要包含2020年07月12日这一天，则应写为`<'2020-07-13'`


# 运算
## 不等于
- 以不等于排除某些记录时，也会排除该字段为空的数据

```sql
SELECT * FROM student
WHERE Sname != '王五';
-- 该查询语句的返回结果不包含包含Sname为空的记录

-- 严谨的写法：
SELECT * FROM student
WHERE Sname != '王五' OR Sname IS NULL;
-- 返回结果不包括Sname为“王五”的记录，但包括Sname为空的记录
```


# 字符串

## REPLACE()
- `LEFT`, `RIGHT`, `SUBSTRING`应习惯性与`REPLACE()`嵌套使用；因为有些数据中间可能含有意料之外的空格，导致字符串切片出错。

```sql
SUBSTRING(REPLACE(字段, ' ', ''), 7, 4)
```

## SUBTRING()
提取字符串的子串

```sql
SUBSTRING(S, 3, 5)  -- 提取字符串S的从左数第3个字母起（包括第3个）的5个字母
SUBSTRING(S, -3)  -- 提取字符串S的从右倒数第3个字母起（包括第3个）、向右的所有的字母
```

{% note default %}
相关在线练习：
- [HackerRank1](https://www.hackerrank.com/challenges/weather-observation-station-6/problem)
- [HackerRank2](https://www.hackerrank.com/challenges/weather-observation-station-7/problem)
- [HackerRank3]((https://www.hackerrank.com/challenges/weather-observation-station-8/problem)
- [HackerRank4](https://www.hackerrank.com/challenges/weather-observation-station-9/problem)
- [HackerRank5](https://www.hackerrank.com/challenges/weather-observation-station-10/problem)
- [HackerRank6](https://www.hackerrank.com/challenges/weather-observation-station-11)
{% endnote %}

# 其他
## SET
`SET @变量名称=变量值`：可以快速替换重复的代码

```sql
SET @DT='2020-07-12';
SELECT * FROM table_1 WHERE date>@DT;
```

## @
`SELECT (@i:=@i+1) AS num `生成一列递增序列 


## 延时注入
MySQL中使用延时注入常用的语句：
- `sleep(5)`
- `BENCHMARK(10000000, MDG(1))`：第一个参数是执行的次数，第二个参数是要执行的函数/表达式

但是使用`BENCHMARK() (M)`会让MySQL停一下，会大量消耗web服务器资源

# 其他注意事项
- 注意字段的格式，数字和字符串不会隐式转换
- 以 MySQL 5.7 或更低版本为准的数据库中，如何正确选择和使用合适的数据类：
  - 更小通常更好
  - 尽量用最简单的数据类型
  - 尽量不适用NULL作为字段值


# 参考资料
- [9 道 MySQL 面试题](https://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457305755&idx=2&sn=764158cf905b0622e1697f51baa718f2&chksm=88a590afbfd219b98bd7d83c2f4ea4d572654b5889eff254bbbb60f2189f61aee393e1801510&mpshare=1&scene=1&srcid=&sharer_sharetime=1590150427096&sharer_shareid=b539221659d6ecf12200314308b58dd3&key=21ed68970b00e8fc75b1e914e6c754e962a5fcec08d9c1e0fb5bfcb82d495aac1ab014508f15a5ebb1eaf6e1c2a2036cecc7516e989e5f1bd3b3615846e67437d7d0ad461bc0ca4d28139dc860680c5b&ascene=1&uin=MjAwNDUzMjgxNw%3D%3D&devicetype=Windows+10+x64&version=62090070&lang=zh_CN&exportkey=AbupEngztSgB4dsmRHvGbpk%3D&pass_ticket=iLa2XiZaCBS2XSrbXAnYFAIdgj9HEiggWqWDoDR8tX5Ckgf6sdWkU3%2BCxbmiJIYM)
- [那些SQL里面踩过的坑](https://mp.weixin.qq.com/s?__biz=MzA3MTg4NjY4Mw==&mid=2457307570&idx=2&sn=c8a3f3f45fa2ed9998be713528272ad9&chksm=88a59b86bfd21290bf9dc79be9bebfe57bffcc1352682b152bb354c7177449021777f6fcb1ca&mpshare=1&scene=24&srcid=&sharer_sharetime=1592959329103&sharer_shareid=b539221659d6ecf12200314308b58dd3&key=41f971365debdfe9cf3f83cd39b1b1e3392a61f125626cee8d56db3cf1d9e8674ec2fb0399a4cedb0f3b12737250e9ccd8423a5f76c71a1e27a8026edf2de8c86538603ff9370d460020772e5dfff60d&ascene=14&uin=MjAwNDUzMjgxNw%3D%3D&devicetype=Windows+10+x64&version=62090529&lang=zh_CN&exportkey=AXttlvqiichHWPtD4HqdOrU%3D&pass_ticket=ws7WEjys7meG8tQvR9%2BINrP5RKEp1Mus1mqNfeaPYCNHYnD2%2FhjK5ZF3mONasy7H)
- [MySQL的information_schema库](https://www.jianshu.com/p/ea15158f39f7)
- [MySQL元数据库——information_schema](https://www.cnblogs.com/postnull/p/6697077.html)
- [MySQL information_schema 详解](https://zhuanlan.zhihu.com/p/88342863)