---
title: SQL
date: 2020-04-30 00:58:41
tags: [SQL,MySQL]
categories: 数据库
mathjax: true
toc: true
index_img: /img/SQL.jpg  ## 封面图片
top: 8
---

<center>SQL相关知识积累~</center>

<!--more-->


# SQL
SQL语言具有两种**使用方式**，分别为
1. **交互式SQL**：在终端交互方式下使用
2. **嵌入式SQL**：嵌入在高级语言的程序中使用；这些高级语言可以是C语言、PASCAL、COBOL等，被称为宿主语言


## 交互式SQL
### MySQL
{% post_link MySQL  %}

### MS SQL Server
{% post_link SQL-Server %}

### ACCESS
{% post_link ACCESS %}


## 嵌入式SQL
### 嵌入C语言程序
- SQL 与 C 语言处理记录的方式是不同的。当将 SQL 语句嵌入到 C 语言程序时，为协调两者而引入**游标**（[题目来源](https://www.nowcoder.com/questionTerminal/56131e315f2d4071847d6a052b47260d)）

# SQL标准
- SQL92标准定义的最严格事务级别是：Serializable（可串行化）

# SQL语言
SQL有四种语言（也有的说法将TCL考虑为第五种）：
- DDL  数据定义语言
- DQL  数据查询语言
- DML  数据库操纵语言
- DCL  数据库控制语言
- TCL  事务控制语言

详情请见《{% post_link sql-sql语言类型 SQL语言类型 %}》

# 相关书籍
- {% post_link sql-sql必知必会 《SQL必知必会》 %} 
- {% post_link sql-SQL进阶教程 《SQL进阶教程》 %}
- {% post_link sql-sql解惑 《SQL解惑》 %}

# 数据设计
- 在关系数据库设计中，关系模式是用来记录用户数据的**二维表**

## 概念结构设计
数据设计中的E-R模型设计是**概念结构设计**阶段的主要工作之一
- 数据库的概念独立于具体的机器和DBMS

### E-R方法
实体-联系方法（E-R，Entity-Relationship Approach），是描述现实世界概念结构模型的有效方法。
- **矩形**：实体型
- **椭圆**：实体的属性
- **菱形**：实体型之间的联系


## 逻辑结构设计
- 在数据库设计中，将ER图转换为关系数据模型的过程属于**逻辑设计阶段**

## 物理结构设计



# 键（码）
数据库中的**键**（key）也可以称为**码**，是关系模型中的一个重要概念，它是逻辑结构，不是数据库的物理部分。

- **候选键（码）**：如果关系中的某一属性组的值能唯一地标识一个元组，则称该属性组为候选码
- **主键（码）**：如果一个关系有多个候选码，则选定其中一个为主码
- **主属性**：候选码的诸属性
- **非主属性**：不包含在任何候选码中的属性

## 超键（码）
能唯一标识元组的属性集中的其中一个属性可以作为一个超键，多个属性组合也可以作为一个超键（Super Key）。
- 在关系中能唯一标识元组的属性集称为关系模式的超键

## 候选键（码）
候选键（码）（Candidate Key）有2个要求：
1. 始终能够唯一地标识一个元组
2. 在属性集中找不出真子集能够满足条件

- 可以将“候选键（码）”理解为不能再“缩小”的超键（不含有多余属性的超键）

## 主键（码）
如果一个关系有多个候选键（码），则选定其中一个为主键（码）（Primary Key）
- 在关系模式中，对应关系的主键必须是能唯一确定元组的一组属性
- 主键是唯一、不为空值的列

## 外键（码）
- 在一个关系A中，有一个属性b不是关系A的主键（码）或候选键（码），但是是另一个关系B的主键，则关系A中的属性b是关系A中的外键（码）
- 在SQL语言中使用`FOREIGN KEY`时，与之配合的语句是`REFERENCES`

## 全键（码）
- 是候选键（码）的一种特殊情况
- 如果关系中只有一个候选键（码），且这个候选键（码）中包含了全部属性，那么称这个候选码为全键（码）


# 事务
详情请见《{% post_link sql-事务 SQL事务 %}》

## 特性
数据库事务的四大特性（简称**ACID**）：
1. 原子性（Atomicity）
2. 一致性（Consistency）
3. 隔离性（Isolation）
4. 持久性（Durability）

## 事务隔离级别
1. Read Uncommitted（未提交读）
2. Read Committed（提交读）
3. Repeatable Read（可重复读）
4. Serializable（可串行化）

- 隔离级别依次增加
- 并发性能依次降低
- 随着隔离级别的增高，并发性能降低

# 视图
- 在视图上能够完成的操作：
  - 更新视图
  - 查询
  - 在视图上定义新的视图
- 在视图上**不能**完成的操作是：在视图上定义新的表



# 数据类型


# 通用

- {% post_link sql-范式 范式 %}
- {% post_link sql-2-模式 模式 %}
- {% post_link sql-3-数据完整性 数据完整性 %}
- {% post_link sql-sql语句执行顺序 SQL语句执行顺序 %}


## COALESCE()
`COALESCE(expression1, expression2, expression3, ...)`：接收一系列表达式或列，返回第一个非空的值。
- 如果都是空值（NULL），则会报错

## CREATE
### CREATE FUNCTION
编写函数：
```sql
-- 某电器商品海关进口税征收办法，起征点为500元，超出部分按以下2级计算：
-- 1 、超过0至150， 税率3%
-- 2、 超过150元以上 ，税率10%
-- 商品进口税=(商品总额-500)*税率
-- 编写一个函数，实现输入商品的总额，返回该商品的进口税
CREATE FUNCTION goods(@total AS money)
RETURNS money AS
BEGIN
  declare @income money
  declare @tax money
SELECT @income = @total - 500
IF @income <= 0 SET @tax = 0
ELSE 
  BEGIN
    IF(0 < @income AND @income <= 150)
      SELECT @tax = @income * 0.03
    IF(@income > 150)
      SELECT @tax = @income * 0.1
    END
RETURN @tax
END

-- 或
CREATE FUNCTION goods(@total AS money)
RETURNS money AS 
BEGIN 
  declare @income money
  declare @tax money
SET @income = @total - 500
IF @income <= 0 SET @tax = 0
ELSE 
  BEGIN 
    IF(0 < @income AND @income <= 150)
      SET @tax = @income * 0.03
    IF(@income > 150)
      SET @tax = @income * 0.1
    END
RETURN @tax
END

```

## DATEDIFF()
`DATEDIFF(string enddate, string startdate)`：返回结束日期减去开始日期的天数
- 返回值：`int`

```sql
-- 计算0701-0715的区间三日留存率
-- 区间三日留存率：t+1、t+2、t+3任意一天有回访即算留存
SELECT 
	  t1.aday
    , ROUND(COUNT(DISTINCT t2.id) / COUNT(DISTINCT t1.id) ,4)AS cohort_rate
FROM cust t1 
LEFT JOIN cust t2
ON t1.id = t2.id
AND (DATEDIFF(t2.aday, t1.aday) =1 OR DATEDIFF(t2.aday, t1.aday) =2 OR DATEDIFF(t2.aday, t1.aday) =3)
WHERE t1.aday BETWEEN '2020-07-01' AND '2020-07-15'
GROUP BY 
	t1.aday;
```

```sql
-- 计算次日、2日、3日留存率
SELECT 
    tt1.date
    , tt2.gap
    , tt2.retention_num
    , tt2.retention_num / tt1.uv_day0 AS cohort_rate
FROM 
(
    SELECT 
        date
        , COUNT(DISTINCT user_id) AS uv_day0
    FROM tbl_new_users
    WHERE date = '2020-07-01'
    GROUP BY date
) AS tt1

LEFT JOIN
(
    SELECT
        t1.date
        , gap
        , COUNT(*) AS retention_num
    FROM 
    (
        SELECT 
            date
            , user_id AS uid1
        FROM tbl_new_users
        WHERE date = '2020-07-01'
    ) AS t1
    LEFT JOIN 
    (
        SELECT
            user_id AS uid2
            , DATEDIFF(date, '2020-07-01') AS gap
        FROM tbl_active_users
        WHERE date BETWEEN '2020-07-01' AND '2020-07-03'
    ) AS t2
    ON t1.uid1 = t2.uid2
    GROUP BY 
        date
        , gap
) tt2
ON tt1.date = tt2.date
ORDER BY 
    tt1.date
    , tt2.gap;
```

## DATE_FORMAT()
以不同的格式显示日期/时间数据
```sql
-- 当月的第一天
DATE_FORMAT('${date}', 'yyyy-MM-01')

-- 每月1号调度任务，任务涉及前一个月的所有数据
pt between date_format(date_sub('${date}', 1), 'yyyy-MM-01') and date_sub('${date}', 1) -- 其中 ${date}是某月1号
-- 上上个自然月
pt between date_format(date_sub(date_format(date_sub('${date}', 1), 'yyyy-MM-01'), 1), 'yyyy-MM-01') and date_sub(date_format(date_sub('${date}', 1), 'yyyy-MM-01'), 1)
```


## DELETE
删除表中数据：`DELETE TABLE table_name`
- [TRUNCATE](#TRUNCATE)也是删除表中数据的语句
  - `TRUNCATE`比`DELETE`的速度快
  - `TRUNCATE`是删除表的所有行；而`DELETE`是删除表的一行或多行（除非没有`WHERE`子句）
  - 在删除时，如果遇到任何一行违反约束（主要是外键约束），则`TRUNCATE`仍然删除表中数据，但是表的结构、列、约束、索引等保持不变，而`DELETE`直接返回错误
  - 对于被外键约束的表，不能使用`TRUNCATE`，而应该使用不带`WHERE`子句的`DELETE`
  - 如果想保留标识计数值，要用`DELETE`；因为`TRUNCATE`会对新行标识所用的计数值重置为该列的种子



## DROP
删除表：`DROP TABLE table_name`

## grouping sets
分组集（Grouping Sets）是多个分组的并集，用于在一个查询中，按照不同的分组列对集合进行聚合运算，等价于对单个分组使用“union all”，计算多个结果集的并集。
- 在单个分组中缺失的分组列，返回`null`

```sql
select  a
        , b
        , c
        , count(1) as cnt 
        , sum(d) as total_d
from    tbl
group by  a
          , b
          , c
grouping sets (a, (a, b), (a, b, c))
;
-- 等价于
select  a
        , null
        , null
        , count(1) as cnt 
        , sum(d) as total_d
from    tbl
group by a

union all

select  a
        , b
        , null 
        , count(1) as cnt 
        , sum(d) as total_d
from    tbl
group by  a, b

union all

select  a
        , b
        , c
        , count(1) as cnt 
        , sum(d) as total_d
from    tbl
group by  a, b, c;
```


## IIF()
`IIF(boolean_expression, valueForTrue, valueForFalse)`：如果`boolean_expression`为真，则返回`valueForTrue`；否则返回`valueForFalse`。

## INTO



## ISNULL()
`ISNULL(expression1, expression2)`：如果第一个参数为`NULL`，则返回第二个参数expression2；否则，返回第一个参数expression1。等价于`CASE WHEN expression1 IS NULL THEN expression2 ELSE expression1 END`。

## LAG()
滞后若干项
```sql
-- 表login_log记录用户的登录行为：uid（用户id）、login_date（登陆日期）
-- 求连续登陆5天的用户id
select uid
from  
(
    select  uid
            , login_date -- 登陆日期
            , lag(login_date, 4) over(partition by uid order by login_date asc) as pre_5_date -- 登陆日期往前4个记录
    from    login_log
) a 
where  date_diff(login_date, pre_5_date) = 4;
```

## last_day()
取某月的最后一天
```sql
select  last_day('2020-11-06');
-- 即 2020-11-30
```

## locate()
`LOCATE(substr, str)`：返回字符串str第一次出现子串substr的位置。
- 如果substr不在str中，则返回0

`LOCATE(substr, str, pos)`：返回第一次出现在字符串str的子串substr的位置，从位置pos开始。
- 如果substr不在str中，则返回0

## MAX()

```sql
-- 用户最近登录的日期
SELECT 
    date AS d
FROM 
(
    SELECT
        user_id
        , date
        , ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY date DESC) AS r_num
    FROM login
) AS t
WHERE r_num = 1;

-- 或
SELECT 
    MAX(date)
FROM login
GROUP BY user_id
ORDER BY user_id;
```

## MIN

- [题源](https://www.nowcoder.com/practice/16d41af206cd4066a06a3a0aa585ad3d?tpId=82&&tqId=35086&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)：
```sql
-- 新登录用户的次日登录成功的留存率
SELECT
    ROUND(1.0 * COUNT(DISTINCT l1.user_id) / (SELECT COUNT(DISTINCT user_id) FROM login), 3)
FROM login l1, login l2
ON l1.user_id = l2.user_id
AND DATE(l1.date, '+1 day') = l2.date
AND l1.date = (SELECT MIN(date) FROM login WHERE user_id = l1.user_id);
```


## NULLIF()
`NULLIF(expression1, expression2)`：如果两个参数相等，则返回`NULL`；否则，返回第一个参数。等价于`CASE WHEN expression1 = expression2 THEN NULL ELSE expression1 END`。
- 可用于防止分母中出现0而报错
```sql
a/NULLIF(b, 0)
-- 当b为0时，该式直接返回NULL
```



## ORDER BY
- 用于排序
- 默认升序`ASC`；降序需要在列名后加`DESC`
- 对带有排序作用的`ORDER BY`子句的查询，`ORDER BY`返回的是一个对象，其中的行按特定的顺序组织在一起，这种对象被称为**游标**
- `ORDER BY`子句是唯一能重用列别名的子句
  - 这是因为`ORDER BY`子句是在`SELECT`子句之后才执行的
  ```sql
  (8)SELECT (9)DISTINCT (11) <TOP num><SELECT list>
  (1)FROM [left_table]
  (3)<JOIN_type> JOIN <right_table>
  (2)ON <join_condition>
  (4)WHERE <where_condition>
  (5)GROUP BY <group_by_list>
  (6)WITH <CUBE | ROLLUP>
  (7)HAVING <having_condition>
  (10)ORDER BY <order_by_list>;
  ```
- 谨慎使用`ORDER BY`后面接数字的方式来进行排序，尽量使用`ORDER BY+列名`或`ORDER BY+列别名`
  ```sql
  SELECT 
    col_a,
    col_b,
    col_c
  FROM table_1
  ORDER BY 1, 2, 3; 
  -- 实际按照SELECT后的字段顺序col_a, col_b, col_c进行排序
  -- 当查询的列发生改变时，而忘了修改ORDER BY后的数字时，可能会获得错误的查询结果
  ```
- 表表达式不能使用`ORDER BY`进行排序
  - **表表达式**包括：
    - 视图（VIEW）
    - 内联表值函数
    - 派生表（子查询）
    - 公用表表达式（CTE）
  - 表表达式加了`TOP`可以使用`ORDER BY`
  ```sql
  SELECT 
    col_a
    , col_b
  FROM 
  (
      SELECT TOP 5 *
      FROM table_1
      ORDER BY col_c
  ) AS t1  -- 实际返回的是没有固定顺序的表
  ORDER BY col_a, col_b DESC;
  ```


## REGEXP_REPLACE()
正则替换

```sql
-- 如何统计表tbl的字段comments中出现了多少次“何时下班”？
-- 同一个句话中可能会出现多个“何时下班”
select  sum(len_1 - len_2)
from
(
    select  comments
            , len(comments) as len_1
            , len(regexp_replace(comments, '何时下班', '何时下')) as len_2
    from    tbl
) a;
```


## ROW_NUMBER()

```sql
-- 给定表table_a（包含user_id, login_date），计算连续3天登录的用户id
SELECT  user_id
FROM 
(
    SELECT  user_id
            , COUNT(1) AS cnt
            , DATE_SUB(login_date, t.rn) flag_dt
    FROM 
    (
        SELECT  user_id
                , login_date
                , ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY login_date) AS rn
        FROM    table_a
    ) t
    GROUP BY  user_id
              , DATE_SUB(login_date, t.rn)
    HAVING COUNT(1) >= 3
);
-- 参考：https://www.jianshu.com/p/9a803a61a145
```

```sql
-- 给定表table_a（包含user_id, login_date），计算最近30天内，曾连续3天登录的用户数
SELECT  COUNT(DISTINCT user_id)
FROM 
(
    SELECT  user_id
            , DATE_SUB(login_date, t.rn) AS flag_dt
            , COUNT(1) AS cnt
    FROM 
    (
        SELECT  user_id
                , login_date
                , ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY login_date) AS rn
        FROM    table_a
        WHERE   login_date BETWEEN ${date-30} AND ${date-1}
    ) t
    GROUP BY  user_id
              , DATE_SUB(login_date, t.rn)
    HAVING    COUNT(1) >= 3
) tt;
```

```sql
-- 表login_log记录用户每天的登陆情况：uid（用户id）、date_dy（日期）、is_login（用户是否登陆，1表示当天有登陆，0表示当天未登陆）
-- 求用户的最大连续登陆天数
select  uid
        , max(cnt) as max_con_login_days -- 最大连续登陆天数
from 
(
    select  uid
            , date_diff
            , count(1) as cnt 
    from  
    (
        select  uid
          , date_dy
          , datediff(date_dy, num) as date_diff 
        from 
        (
            select  uid
                    , date_dy
                    , row_number() over(partition by uid order by date_dy asc) as num 
            from    login_log
            where   is_login = 1
        ) a 
    ) b
    group by  uid
              , daet_diff 
) c 
group by uid;
```

```sql
-- 连续四天看相同类别的视频的用户id及视频id
SELECT  user_id
        , video_id
FROM
(
    SELECT  user_id
            , video_id
            , video_category
            , rn_2 - rn_1 AS rn
    FROM 
    (
        SELECT  user_id
                , video_id
                , video_category
                , ROW_NUMBER() OVER(PARTITION BY user_id, video_category ORDER BY time) AS rn_1
                , ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY time) AS rn_2
        FROM    table_a
    ) AS t1
    GROUP BY  user_id
              , video_category
    HAVING COUNT(1) >= 4
) AS t2;
-- 如果连续时间看的视频类别一样，rn_2 - rn_1的差值一样
```

## SUM
求和
- 聚合函数之一

```sql
-- 求累计薪资和
SELECT  emp_no
        , salary
        , SUM(salary) OVER(ORDER BY emp_no) AS running_total
FROM    salaries
WHERE   to_date = '9999-01-01'
;
```

[题源](https://www.nowcoder.com/practice/e524dc7450234395aa21c75303a42b0a?tpId=82&&tqId=35087&rp=1&ru=/ta/sql&qru=/ta/sql/question-ranking)：
```sql
-- 统计每天的新用户
SELECT  date
        , SUM(CASE WHEN r_num = 1 THEN 1 ELSE 0 END) AS new
FROM 
(
    SELECT  date
            , ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY date ASC) AS r_num
    FROM    login
) t1
GROUP BY date;
```



# 参考资料
- [ORDER BY的用法](https://mp.weixin.qq.com/s/5iv1z6SCf0nMyIEWavlyRQ)
- [数据库中的键（码）](https://blog.csdn.net/xyzyhs/article/details/99438912)
- [TSQL 分组集（Grouping Sets）](https://www.cnblogs.com/ljhdo/p/5056757.html)
- [Hive分析窗口函数(五) GROUPING SETS,GROUPING__ID,CUBE,ROLLUP](http://lxw1234.com/archives/2015/04/193.htm)