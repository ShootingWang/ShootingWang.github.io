---
title: SQL | 《SQL进阶教程》
date: 2020-05-12 22:52:46
tags: [SQL]
categories: [数据库]
mathjax: true
toc: true
hide: true
notshow: true
index_img: /img/SQL.jpg  ## 封面图片
---

<center>《SQL进阶教程》读书笔记</center>
<!--more-->


# CASE表达式
CASE表达式：
- 简单CASE表达式（simple case expression）
- 搜索CASE表达式（searched case expression）

```sql
-- 简单CASE表达式
CASE sex
    WHEN '1' THEN 'male'
    WHEN '2' THEN 'female'
ELSE '其他' END

-- 搜索CASE表达式
CASE WHEN sex = '1' THEN 'male'
     WHEN sex = '2' THEN 'female'
ELSE '其他' END
```

- 注意：使用WHEN子句时需要注意条件的排他性
- 一定要注意CASE表达式里每个分支返回的数据类型是否一致
 - 某个分支返回字符型，而其他分支返回数值型的写法是错误的
- CASE表达式末尾不要忘记写END
- ELSE子句是可选的
 - 不写ELSE子句时，CASE表达式的执行结果是NULL
 - 不写ELSE子句可能会造成“语法没有错误，结果却不对”
- 最好明确地写上ELSE子句
- CASE表达式是一种表法式而不是语句

```sql
+-----------+------------+
| pref_name | population |
+-----------+------------+
| 东京      |        400 |
| 佐贺      |        100 |
| 德岛      |        100 |
| 爱媛      |        150 |
| 福冈      |        300 |
| 群马      |         50 |
| 长崎      |        200 |
| 香川      |        200 |
| 高知      |        200 |
+-----------+------------+
9 rows in set (0.00 sec)


-- 根据县人口统计表，统计不同地区的人口数量
SELECT CASE pref_name
                WHEN '德岛' THEN '四国'
                WHEN '香川' THEN '四国'
                WHEN '爱媛' THEN '四国'
                WHEN '高知' THEN '四国'
                WHEN '福冈' THEN '九州'
                WHEN '长崎' THEN '九州'
                WHEN '佐贺' THEN '九州'
        ELSE '其他' END AS district,
        SUM(population)
FROM PopTbl
GROUP BY CASE pref_name
                WHEN '德岛' THEN '四国'
                WHEN '香川' THEN '四国'
                WHEN '爱媛' THEN '四国'
                WHEN '高知' THEN '四国'
                WHEN '福冈' THEN '九州'
                WHEN '长崎' THEN '九州'
                WHEN '佐贺' THEN '九州'
         ELSE '其他' END;
-- 注意是对转换后的列 GROUP

SELECT CASE pref_name
                WHEN '德岛' THEN '四国'
                WHEN '香川' THEN '四国'
                WHEN '爱媛' THEN '四国'
                WHEN '高知' THEN '四国'
                WHEN '福冈' THEN '九州'
                WHEN '长崎' THEN '九州'
                WHEN '佐贺' THEN '九州'
        ELSE '其他' END AS district,
        SUM(population)
FROM PopTbl
GROUP BY district;
-- 这里引用了SELECT子句中定义的别名来 GROUP
-- 与上面那种方法相比，这里不需要写两段CASE WHEN；
-- 而上面的需要写两段CASE WHEN，当CASE内容需要修改时，需要修改两处，容易出错
-- 但是该写法违反标准SQL的规则，因为GROUP BY子句比SELECT语句先执行
```
CASE WHEN 聚合之后：
```sql
+----------+-----------------+
| district | SUM(population) |
+----------+-----------------+
| 九州     |             600 |
| 其他     |             450 |
| 四国     |             650 |
+----------+-----------------+
```

进行不同条件的统计：
```sql
+-----------+-----+------------+
| pref_name | sex | population |
+-----------+-----+------------+
| 东京      | 1   |        250 |
| 东京      | 2   |        150 |
| 佐贺      | 1   |         20 |
| 佐贺      | 2   |         80 |
| 德岛      | 1   |         60 |
| 德岛      | 2   |         40 |
| 爱媛      | 1   |        100 |
| 爱媛      | 2   |         50 |
| 福冈      | 1   |        100 |
| 福冈      | 2   |        200 |
| 长崎      | 1   |        125 |
| 长崎      | 2   |        125 |
| 香川      | 1   |        100 |
| 香川      | 2   |        100 |
| 高知      | 1   |        100 |
| 高知      | 2   |        100 |
+-----------+-----+------------+
16 rows in set (0.00 sec)
-- 如果上表有新列“性别”sex，population列为某县的某个性别人口数
SELECT pref_name,
-- 男性人口
    SUM(CASE WHEN sex='1' THEN population ELSE 0 END) AS cnt_m,
-- 女性人口
    SUM(CASE WHEN sex='2' THEN population ELSE 0 END) AS cnt_f
FROM PopTbl2
GROUP BY pref_name;
+-----------+-------+-------+
| pref_name | cnt_m | cnt_f |
+-----------+-------+-------+
| 东京      |   250 |   150 |
| 佐贺      |    20 |    80 |
| 德岛      |    60 |    40 |
| 爱媛      |   100 |    50 |
| 福冈      |   100 |   200 |
| 长崎      |   125 |   125 |
| 香川      |   100 |   100 |
| 高知      |   100 |   100 |
+-----------+-------+-------+
8 rows in set (0.00 sec)
```

使用CHECK约束定义多个列的条件关系：
```sql
CONSTRAINT check_salary CHECK
        (CASE WHEN dept='IT' 
              THEN CASE WHEN salary>=20000
                        THEN 1 ELSE 0 END
              ELSE 1 END = 1)
-- 如果是IT部门的员工，则工资应大于等于2万；如果不是IT部门的员工，则工资不受该约束限制
-- 蕴含式（conditional）

-- 如果用逻辑与（logical product）
CONSTRAINT check_salary CHECK 
            (dept='IT' AND salary>=20000)
-- 表示 salary里的记录只能是'IT'部门的，且工资大于等于2万
```

在UPDATE语句里进行条件分支：
```sql
+--------+--------+
| name   | salary |
+--------+--------+
| 木村   | 220000 |
| 相田   | 300000 |
| 神崎   | 270000 |
| 齐藤   | 290000 |
+--------+--------+
4 rows in set (0.01 sec)

UPDATE Salaries 
   SET salary = CASE WHEN salary>=300000 THEN salary*0.9
                     WHEN salary>=250000 AND salary<280000 THEN salary*1.2
                     ELSE salary END;  -- 最后一句很重要
-- 当前工资为30万元以上的员工，降薪10%；
-- 当前工资为25万元~28万元（不满28万元）的员工，加薪20%
+--------+--------+
| name   | salary |
+--------+--------+
| 木村   | 220000 |
| 相田   | 270000 |
| 神崎   | 324000 |
| 齐藤   | 290000 |
+--------+--------+
4 rows in set (0.00 sec)
```

调换主键值：
```sql
+-------+-------+-------+
| p_key | col_1 | col_2 |
+-------+-------+-------+
| a     |     1 | 一    |
| b     |     2 | 二    |
| c     |     3 | 三    |
+-------+-------+-------+
3 rows in set (0.00 sec)

UPDATE SomeTable
   SET p_key = CASE WHEN p_key='a' THEN 'b'
                    WHEN p_key='b' THEN 'a'
                    ELSE p_key END   -- 该句很重要
WHERE p_key IN ('a', 'b');
-- 在PostgreSQL和MySQL中执行该语句，会因为主键重复而报错
-- 如: ERROR 1062 (23000): Duplicate entry 'b' for key 'PRIMARY'
```

在CASE表达式中，可以使用`BETWEEN`、`LIKE` 和 `<`、`>`等谓词组合，以及能嵌套子查询的 `IN` 和 `EXISTS` 谓词
```sql
mysql> SELECT * FROM CourseMaster;
+-----------+--------------+
| course_id | course_name  |
+-----------+--------------+
|         1 | 会计入门     |
|         2 | 财务知识     |
|         3 | 簿记考试     |
|         4 | 税务师       |
+-----------+--------------+
4 rows in set (0.00 sec)

mysql> SELECT * FROM OpenCourses;
+--------+-----------+
| month  | course_id |
+--------+-----------+
| 200706 |         1 |
| 200706 |         3 |
| 200706 |         4 |
| 200707 |         4 |
| 200708 |         2 |
| 200708 |         4 |
+--------+-----------+
6 rows in set (0.00 sec)

-- 如果用 IN 谓词
SELECT course_name,
       CASE WHEN course_id IN 
                     (SELECT course_id FROM OpenCourses 
                      WHERE month = 200706) THEN '⚪'
            ELSE '×' END AS "6月",
       CASE WHEN course_id IN 
                     (SELECT course_id FROM OpenCourses
                      WHERE month = 200707) THEN '⚪'
            ELSE '×' END AS "7月",  
       CASE WHEN course_id IN 
                     (SELECT course_id FROM OpenCourses
                      WHERE month = 200708) THEN '⚪'
            ELSE '×' END AS "8月"
FROM CourseMaster;   

-- 使用EXISTS谓词
SELECT CM.course_name,
       CASE WHEN EXISTS
                   (SELECT course_id FROM OpenCourses OC
                    WHERE OC.month=200706 
                      AND OC.course_id=CM.course_id) THEN 'O'
             ELSE 'X' END AS '6月',
        CASE WHEN EXISTS
                    (SELECT course_id FROM OpenCourses OC
                     WHERE OC.month=200707
                       AND OC.course_id=CM.course_id) THEN 'O'
             ELSE 'X' END AS '7月',
         CASE WHEN EXISTS
                     (SELECT course_id FROM OpenCourses OC
                     WHERE OC.month=200708
                       AND OC.course_id=CM.course_id) THEN 'O'
             ELSE 'X' END AS '8月'         
FROM CourseMaster CM;
```

CASE表达式用在SELECT子句里时，既可以写在聚合函数内部，也可以写在聚合函数外部。
```sql
## StudentClub
+--------+---------+-----------+---------------+
| std_id | club_id | club_name | main_club_flg |
+--------+---------+-----------+---------------+
|    100 |       1 | 棒球      | Y             |
|    100 |       2 | 管弦乐    | N             |
|    200 |       2 | 管弦乐    | N             |
|    200 |       3 | 羽毛球    | Y             |
|    200 |       4 | 足球      | N             |
|    300 |       4 | 足球      | N             |
|    400 |       5 | 游泳      | N             |
|    500 |       6 | 围棋      | N             |
+--------+---------+-----------+---------------+
8 rows in set (0.00 sec)
-- main_club_flg='Y' 表示加入多个社团的学生的主社团

-- 找出学生的主社团
SELECT std_id,
        CASE WHEN COUNT(*) = 1
              THEN MAX(club_id)
              ELSE MAX(CASE WHEN main_club_flg = 'Y'
                            THEN club_id
                            ELSE NULL END)
              END AS main_club
FROM StudentClub
GROUP BY std_id;

+--------+-----------+
| std_id | main_club |
+--------+-----------+
|    100 |         1 |
|    200 |         3 |
|    300 |         4 |
|    400 |         5 |
|    500 |         6 |
+--------+-----------+
5 rows in set (0.00 sec)
```

## 练习
找最大值
```sql
-- Greatests
+------+---+---+---+
| key0 | x | y | z |
+------+---+---+---+
| A    | 1 | 2 | 3 |
| B    | 5 | 5 | 2 |
| C    | 4 | 7 | 1 |
| D    | 3 | 3 | 8 |
+------+---+---+---+
4 rows in set (0.00 sec)

-- 找出多列数据中的最大值
/* 找出x列和y列中的最大值 */
SELECT key0,
       CASE WHEN x < y THEN y
            ELSE x END AS greatest
FROM Greatests;

-- MySQL自带函数 GREATEST 直接可实现上述过程
SELECT key0,  GREATEST(x,y) AS greatest
FROM Greatests;

+------+----------+
| key0 | greatest |
+------+----------+
| A    |        2 |
| B    |        5 |
| C    |        7 |
| D    |        3 |
+------+----------+
4 rows in set (0.00 sec)
```

求和：
```sql
+-----------+-----+------------+
| pref_name | sex | population |
+-----------+-----+------------+
| 东京      | 1   |        250 |
| 东京      | 2   |        150 |
| 佐贺      | 1   |         20 |
| 佐贺      | 2   |         80 |
| 德岛      | 1   |         60 |
| 德岛      | 2   |         40 |
| 爱媛      | 1   |        100 |
| 爱媛      | 2   |         50 |
| 福冈      | 1   |        100 |
| 福冈      | 2   |        200 |
| 长崎      | 1   |        125 |
| 长崎      | 2   |        125 |
| 香川      | 1   |        100 |
| 香川      | 2   |        100 |
| 高知      | 1   |        100 |
| 高知      | 2   |        100 |
+-----------+-----+------------+
16 rows in set (0.00 sec)

SELECT sex AS '性别',
       SUM(population) AS '全国',
       SUM(CASE WHEN pref_name = '德岛' THEN population ELSE 0 END) AS "德岛",
       SUM(CASE WHEN pref_name = '香川' THEN population ELSE 0 END) AS "香川",
       SUM(CASE WHEN pref_name = '爱媛' THEN population ELSE 0 END) AS "爱媛",
       SUM(CASE WHEN pref_name = '高知' THEN population ELSE 0 END) AS "高知",
       SUM(CASE WHEN pref_name IN ('德岛', '香川', '爱媛', '高知')
                THEN population ELSE 0 END) AS zaijie
  FROM PopTbl2
 GROUP BY sex;

+--------+--------+--------+--------+--------+--------+--------+
| 性别   | 全国   | 德岛   | 香川   | 爱媛   | 高知   | zaijie |
+--------+--------+--------+--------+--------+--------+--------+
| 1      |    855 |     60 |    100 |    100 |    100 |    360 |
| 2      |    845 |     40 |    100 |     50 |    100 |    290 |
+--------+--------+--------+--------+--------+--------+--------+
2 rows in set (0.00 sec)
```

乱序排序：
```sql

-- 按照 B-A-D-C排序
SELECT key0,
       CASE key0
         WHEN 'B' THEN 1
         WHEN 'A' THEN 2
         WHEN 'D' THEN 3
         WHEN 'C' THEN 4
         ELSE NULL END AS sort_col
  FROM Greatests
 ORDER BY sort_col;

+------+----------+
| key0 | sort_col |
+------+----------+
| B    |        1 |
| A    |        2 |
| D    |        3 |
| C    |        4 |
+------+----------+
4 rows in set (0.00 sec)
```

按照给定的三边长度，判断是否能够组成一个三角形；若能组成三角形，判断三角形的类型。
[题目详情](https://www.hackerrank.com/challenges/what-type-of-triangle/problem)

```sql
-- 注意语句的顺序
SELECT
    CASE WHEN (A+B<=C OR A+C<=B OR B+C<=A) THEN 'Not A Triangle'
         WHEN (A=B AND B=C) THEN 'Equilateral'
         WHEN (A=B OR A=C OR B=C) THEN 'Isosceles'
         ELSE 'Scalene' END AS triangle
FROM TRIANGLES;
```

[HackerRank-Occupations](https://www.hackerrank.com/challenges/occupations/problem)
`CASE WHEN`和RowNumber的用法：
```sql
SET @r1=0, @r2=0, @r3=0, @r4=0;
SELECT MIN(Doctor), MIN(Professor), MIN(Singer), MIN(Actor)
FROM (
    SELECT CASE Occupation
        WHEN "Doctor" THEN (@r1:=@r1+1)
        WHEN "Professor" THEN (@r2:=@r2+1)
        WHEN "Singer" THEN (@r3:=@r3+1)
        WHEN "Actor" THEN (@r4:=@r4+1)
        END AS rownumber,
        CASE WHEN Occupation="Doctor" THEN Name END AS Doctor,
        CASE WHEN Occupation="Professor" THEN Name END AS Professor,
        CASE WHEN Occupation="Singer" THEN Name END AS Singer,
        CASE WHEN Occupation="Actor" THEN Name END AS Actor
    FROM OCCUPATIONS
    ORDER BY Name
) AS TEMP
GROUP BY rownumber;
-- min()/max() will return a name for specific index and specific occupation. If there is a name, it will return it, if not, return NULL.
```

# 排序
**窗口函数**：也称**OLAP函数**、**分析函数**。
- `RANK()`
- `DENSE_RANK()`

```sql
SELECT 
    name
    , price
    , RANK() OVER(ORDER BY price DESC) AS rank_1
    , DENSE_RANK() OVER(ORDER BY price DESC) AS rank_2
FROM Products;

-- +------+-------+--------+--------+
-- | name | price | rank_1 | rank_2 |
-- +------+-------+--------+--------+
-- | 橘子 |   100 |      1 |      1 |
-- | 西瓜 |    80 |      2 |      2 |
-- | 苹果 |    50 |      3 |      3 |
-- | 香蕉 |    50 |      3 |      3 |
-- | 葡萄 |    50 |      3 |      3 |
-- | 柠檬 |    30 |      6 |      4 |
-- +------+-------+--------+--------+
```

> MySQL不支持`RANK()`、`DENSE_RANK()`

```sql
-- RANK()等价于
SELECT 
    t1.name
    , t1.price
    , 
    (
        SELECT COUNT(t2.price)
        FROM Products t2
        WHERE t2.price > t1.price
    ) + 1 AS rank_1
FROM Products t2
ORDER BY rank_1;
-- 排序从1开始
-- 如果出现相同的位次，则跳过之后的位次

-- 或者
SELECT 
    t1.name,
    MAX(t1.price) AS price,
    COUNT(t2.name) + 1 AS rank_1
FROM Products t1 LEFT OUTER JOIN Products t2
ON t1.price < t2.price
GROUP BY t1.name
ORDER BY rank_1;
```

```sql
-- DENSE_RANK()等价于
SELECT 
    t1.name
    , t1.price
    , 
    (
        SELECT COUNT(DISTINCT t2.price)
        FROM Products t2
        WHERE t2.price > t1.price
    ) + 1 AS rank_2
FROM Products t2
ORDER BY rank_2;
```

# 自连接
- 自连接经常与非等值连接结合使用
- 自连接和GROUP BY结合使用可生成递归集合
- 自连接的性能开销大，应尽量给用于连接的列建立索引

# 三值逻辑和NULL
大多数编程语言是基于二值逻辑的——逻辑值只有真和假；而SQL语言则是采用三值逻辑（three-valued logic）——逻辑值除了真和假，还有“不确定”（**unknown**）。

- `NULL`不是值
  - 因此不能对其使用谓词
- 对`NULL`使用谓词后的结果是unknown

## NULL
数据库中存在两种`NULL`，分别指
- 未知（unknown）：虽然现在不知道，但加上某些条件后就可以知道
- 不适用（not applicable，inapplicable）：无论怎么努力都无法知道

<meta name="referrer" content="no-referrer" />
{% asset_img NULL_lost.png 丢失的信息的分类 %}

{% note default %}
**为什么必须写成`IS NULL`，而不是`=NULL`?**

因为对`NULL`使用比较谓词后得到的结果总是unknown，而SQL查询结果只会包含`WHERE`子句中判断结果为true的行，不会包含判断结果为false或unknown的行。因此使用`xxx = NULL`并不能得到正确的查询结果
{% endnote %}

- `NULL`既不是值（value）也不是变量（variable）
- `NULL`只是一个表示“没有值”的标记，而比较谓词只适用于值
- 应把`IS NULL`看作是一个谓词

## unknown
- unknown是第三个逻辑真值
- 真值**unknown**和作为NULL的一种unknown是不同的东西
  - 真值**unknown**是明确的布尔型（boolean）的真值
  - 作为NULL的未知既不是值也不是变量

```sql
-- 以下式子都会被判为unknown
1 = NULL
2 > NULL
3 < NULL
4 <> NULL
NULL = NULL
```
三值逻辑的真值表（NOT）:

|x|NOT x|
|:--:|:--:|
|true|false|
|unknown|unknown|
|false|true|

三值逻辑的真值表（AND）:

|AND|true|unknown|false|
|:--:|:--:|:--:|:--:|
|true|true|unknown|false|
|unknown|unknown|unknown|false|
|false|false|false|false|

三值逻辑的真值表（OR）:

|OR|true|unknown|false|
|:--:|:--:|:--:|:--:|
|true|true|true|true|
|unknown|true|unknown|unknown|
|false|true|unknown|false|

- `AND`中的优先级：`false` > `unknown` > `true`
- `OR`中的优先级：`true` > `unknown` > `false`

```sql
-- a = 2, b = 4, c = NULL
a < b AND b > c 
-- true AND unknown = unknown

a > b OR b < c  
-- false OR unknown = unknown

a < b OR b < c  
-- true OR unknown = true

NOT (b <> c)    
-- NOT(unknown) = unknown
```

{% note warning %}
**排中律（Law of Excluded Middle）**：
命题“把命题和它的否命题通过OR连接而成的命题全都是真命题”在二值逻辑中被称为排中律。
{% endnote %}

- 在SQL中，排中律不成立

```sql
-- 查询年龄是25岁或不是25岁的学生
SELECT * 
FROM Students
WHERE age = 25 
OR age <> 25;
-- 查询结果不包含年龄为空的行

SELECT * 
FROM Students
WHERE age = NULL
OR age <> NULL;
-- 如此才能详尽
-- 或
SELECT * 
FROM Students
WHERE age = 25
OR age <> 25
OR age IS NULL;
```

## CASE 表达式与NULL
`CASE`表达式与`NULL`：
```sql
CASE col_1
    WHEN 1      THEN 'O'
    WHEN NULL   THEN 'X'
END
-- 这个CASE表达式一定不会返回X
-- 因为第二个WHEN子句表示的是col_1=NULL，这个式子的真值永远是unknown
-- CASE表达式只认可真值为true的条件
-- 正确的写法是
CASE
    WHEN col_1 = 1      THEN 'O'
    WHEN col_2 IS NULL  THEN 'X'
END
```

## NOT IN与NOT EXISTS
在SQL中，通常会将`IN`改写成`EXISTS`进行性能优化。但是将`NOT IN`改写成`NOT EXISTS`时，结果未必一样。

```sql
-- 先构建两个表格
CREATE TABLE IF NOT EXISTS class_a (
    name VARCHAR(20),
    age INT(5),
    city VARCHAR(40)
);

INSERT INTO class_a VALUES ('布朗', 22, '东京');
INSERT INTO class_a VALUES ('拉里', 19, '埼玉');
INSERT INTO class_a VALUES ('伯杰', 21, '千叶');

CREATE TABLE IF NOT EXISTS class_b(
    name VARCHAR(20),
    age INT(5),
    city VARCHAR(40)
);

INSERT INTO class_b VALUES ('齐藤', 22, '东京');
INSERT INTO class_b VALUES ('田尻', 23, '东京');
INSERT INTO class_b(name, city) VALUES ('山田', '东京');  --age为空
INSERT INTO class_b VALUES ('和泉', 18, '千叶');
INSERT INTO class_b VALUES ('武田', 20, '千叶');
INSERT INTO class_b VALUES ('石川', 19, '神奈川');

SELECT * FROM class_a;
-- +--------+------+--------+
-- | name   | age  | city   |
-- +--------+------+--------+
-- | 布朗   |   22 | 东京   |
-- | 拉里   |   19 | 埼玉   |
-- | 伯杰   |   21 | 千叶   |
-- +--------+------+--------+

SELECT * FROM class_b;
-- +--------+------+-----------+
-- | name   | age  | city      |
-- +--------+------+-----------+
-- | 齐藤   |   22 | 东京      |
-- | 田尻   |   23 | 东京      |
-- | 山田   | NULL | 东京      |
-- | 和泉   |   18 | 千叶      |
-- | 武田   |   20 | 千叶      |
-- | 石川   |   19 | 神奈川    |
-- +--------+------+-----------+
```

`NOT IN`子查询用到的表里被选择的列中存在`NULL`，则SQL语句整体的查询结果永远是空。

```sql
-- 查找与B班住在东京的学生年龄不同的A班学生
SELECT * 
FROM class_a
WHERE age NOT IN (
    SELECT age
    FROM class_b
    WHERE city='东京'
);
-- Empty set (0.01 sec)
-- 查询结果为空

-- 分步骤查找原因：
-- 1. 获取年龄列表
SELECT * 
FROM class_a
WHERE age NOT IN (22, 23, NULL);
-- Empty set (0.00 sec)

-- 2. 用NOT和IN改写NOT IN
SELECT *
FROM class_a
WHERE NOT age IN (22, 23, NULL);
-- Empty set (0.00 sec)

-- 3. 用OR等价改写IN
SELECT * 
FROM class_a
WHERE NOT ( (age = 22) OR (age = 23) OR (age = NULL));
-- Empty set (0.00 sec)

-- 4. 使用De Morgan定律等价改写
SELECT * 
FROM class_a
WHERE NOT (age = 22) AND NOT (age = 23) AND NOT (age = NULL);
-- Empty set (0.00 sec)

-- 5. 用<>等价改写NOT 和 = 
SELECT * 
FROM class_a
WHERE (age <> 22) AND (age <> 23) AND (age <> NULL);
-- Empty set (0.00 sec)
-- 对NULL使用<>后，结果为unknown
-- unknown AND unknown = unknown
```

应该使用`EXISTS`谓词：
```sql
SELECT * 
FROM class_a a
WHERE NOT EXISTS (
    SELECT *
    FROM class_b b
    WHERE a.age=b.age
    AND b.city='东京'
);
-- +--------+------+--------+
-- | name   | age  | city   |
-- +--------+------+--------+
-- | 拉里   |   19 | 埼玉   |
-- | 伯杰   |   21 | 千叶   |
-- +--------+------+--------+
-- 2 rows in set (0.00 sec)
```

- `EXISTS`谓词只会返回true或false，而不会返回unknown
- `IN`和`EXISTS`可以互相替换使用，而`NOT IN`和`NOT EXISTS`却不可以互相替换

## 限定谓词与NULL
- SQL中有`ALL`和`ANY`两个限定谓词
  - `ANY`和`IN`是等价的
  - `ALL`可以与比较谓词一起使用
  - `ALL`谓词其实是多个以`AND`连接的逻辑表达式的省略写法，如果`ALL()`的输入中包含NULL，则查询结果为空
- 当表里存在`NULL`时，限定谓词与极值谓词不等价
- 极值谓词（`MIN`、`MAX`）在输入为空表（空集）时会返回`NULL`
- 当需要返回所有行时，需要使用`ALL`谓词，或者使用`COALESCE`函数将极值函数返回的`NULL`处理成合适的值

# HAVING子句
- `WHERE`子句用来调查集合元素的性质，而`HAVING`子句用来调查集合本身的性质

```sql
-- 查询编号是否连续
SELECT  '存在缺失的编号' AS gap 
FROM    tbl
HAVING  COUNT(1) <> MAX(seq);
-- 检查自然数集合和tbl集合之间是否存在一一映射
-- 上述语句没有GROUP BY子句，整张表会被聚合为一行

-- 查询缺失编号的最小值
SELECT  MIN(seq + 1) AS gap
FROM    tbl
WHERE   (seq + 1) NOT IN (SELECT seq FROM tbl);
-- 如果tbl里包含NULL，这条SQL语句的查询结果就不正确了
```

- 以前的SQL标准中，`HAVING`子句必须和`GROUP BY`子句一起使用；按照现在的SQL标准，`HAVING`子句可以单独使用，但是不能在`SELECT`子句里引用原来的表里的列

## 求众数
```sql
-- 求众数
-- 众数：反映群体趋势
SELECT  income
        , COUNT(*)
FROM    Graduates
GROUP BY    income
HAVING  COUNT(*) >= ALL(
    SELECT  COUNT(*)
    FROM    Graduates
    GROUP BY    income
);
-- ALL用于NULL或空集时会出现问题，可用极值函数来代替
SELECT  income
        , COUNT(*) AS cnt
FROM    Graduates
GROUP BY income
HAVING COUNT(*) >= (
    SELECT  MAX(cnt)
    FROM (
        SELECT  COUNT(*) AS cnt
        FROM Graduates 
        GROUP BY income
    ) tmp
);
```

## 求中位数
```sql
-- 求中位数
-- 在HAVING子句中使用非等值自连接
SELECT  AVG(DISTINCT income)
FROM    
(
    SELECT  T1.income
    FROM    Graduates T1, Graduates T2
    GROUP BY T1.income
    HAVING  SUM(CASE WHEN T2.income >= T1.income THEN 1 ELSE 0 END) >= COUNT(*) / 2
    AND     SUM(CASE WHEN T2.income <= T1.income THEN 1 ELSE 0 END) >= COUNT(*) / 2
) TMP;
```

## COUNT(*) vs COUNT(列名)

||COUNT(*)|COUNT(列名)|
|:--:|:----|:-----|
|NULL|统计NULL|不统计NULL|
||查询的是所有行的数目|排除掉NULL值|

```sql
-- 查询“提交日期”列内不包含NULL的学院
SELECT  dpt
FROM    students
GROUP BY    dpt
HAVING      COUNT(*) = COUNT(smbt_date);
-- 或
SELECT  dpt -- 学院名
FROM    students
GROUP BY    dpt
HAVING  COUNT(*) = SUM(
    CASE WHEN sbmt_date IS NOT NULL 
    THEN 1 ELSE 0 END
);
```

```sql
-- 购物篮分析
-- 找出shopitems表中同时包含items表中物品的商铺
-- 带余除法（division with a remainder）
SELECT  SI.shop
FROM    ShopItems SI, Items I
WHERE   SI.item = I.item
GROUP BY    SI.shop
HAVING  COUNT(SI.item) = (
    SELECT COUNT(item) FROM Items
);

-- 找出ShopItems表中包含Items表中所有物品的商铺（物品完全一致）
-- 精确关系除法（exact relational division）
SELECT  SI.shop
FROM    ShopItems SI 
LEFT OUTER JOIN Items I
ON      SI.item = I.item
GROUP BY SI.shop
HAVING  COUNT(SI.item) = (SELECT COUNT(item) FROM Items)
AND     COUNT(I.item) = (SELECT COUNT(item) FROM Items);
```

# 外连接
## 行列转换：行转列
行列转换求交叉表：
```sql
-- 水平展开1：使用外连接
SELECT  c0.name
        , CASE WHEN c1.name IS NOT NULL THEN 'O'
            ELSE NULL END AS `SQL入门`
        , CASE WHEN c2.name IS NOT NULL THEN 'O'
            ELSE NULL END AS `UNIX基础`
        , CASE WHEN c3.name IS NOT NULL THEN 'O'
            ELSE NULL END AS `Java中级`
FROM 
(
    SELECT  DISTINCT name 
    FROM    Courses
) c0
LEFT OUTER JOIN
(
    SELECT  name
    FROM    Courses 
    WHERE   course = 'SQL入门'
) c1
ON  c0.name = c1.name

LEFT OUTER JOIN
(
    SELECT  name
    FROM    Courses 
    WHERE   course = 'UNIX基础'
) c2 
ON  c0.name = c2.name

LEFT OUTER JOIN
(
    SELECT  name
    FROM    Courses
    WHERE   course = 'Java中级'
) c3
ON  c0.name = c3.name;
```

- 一般情况下，外连接可以用标量子查询替代
- 但是在`SELECT`子句中使用标量子查询，性能消耗大
```sql
-- 水平展开2：使用标量子查询
SELECT  c0.name
        , (
            SELECT  'O'
            FROM    Courses c1
            WHERE   course = 'SQL入门'
            AND     c1.name = c0.name
        ) AS `SQL入门`
        , (
            SELECT  'O'
            FROM    Courses c2
            WHERE   course = 'UNIX基础'
            AND     c2.name = c0.name
        ) AS `UNIX基础`
        , (
            SELECT  'O' 
            FROM    Courses c3
            WHERE   course = 'Java中级'
            AND     c3.name = c0.name
        ) AS `Java中级`
FROM
(
    SELECT  DISTINCT name
    FROM    Courses
) c0;
```

- `CASE`表达式可以写在`SELECT`子句的聚合函数内部或外部
```sql
-- 水平展开3：嵌套使用CASE表达式
SELECT  name
        , CASE WHEN SUM(CASE WHEN course = 'SQL入门' THEN 1 ELSE NULL END) = 1 THEN 'O'
            ELSE NULL END AS `SQL入门`
        , CASE WHEN SUM(CASE WHEN course = 'UNXI基础' THEN 1 ELSE NULL END) = 1 THEN 'O'
            ELSE NULL END AS `UNIX基础`
        , CASE WHEN SUM(CASE WHEN course = 'Java中级' THEN 1 ELSE NULL END) = 1 THEN 'O'
            ELSE NULL END AS `Java中级`
FROM    Courses
GROUP BY    name;
```

## 行列转换：列转行

```sql
-- 列转行：使用UNION ALL
SELECT  employee
        , child_1 AS child
FROM    Personnel
UNION ALL
SELECT  employee
        , child_2 AS child
FROM    Personnel
UNION ALL
SELECT  employee
        , child_3 AS child
FROM    Personnel;
-- UNION ALL不会排除掉重复的行，可能会出现child列为NULL的情况
```

外连接：
- 左外连接`LEFT OUTER JOIN`
- 右外连接`RIGHT OUTER JOIN`
- 全外连接`FULL OUTER JOIN`：把两张表都当作主表来使用

> - 左外连接和右外连接没有功能上的区别：用作主表的表写在运算符左边时用左外连接，写在运算符右边时写右外连接
- 如果所用的数据库不支持全外连接，可以分别进行左外连接和右外连接，再把两个结果通过`UNION`合并起来
- MySQL不支持全外连接
- `OUTER`可省略

```sql
-- MySQL全外连接
SELECT  a.id AS id
        , a.name
        , b.name
FROM    Class_a a 
LEFT OUTER JOIN Class_b b
ON  a.id = b.id

UNION 

SELECT  b.id AS id
        , a.name
        , b.name
FROM    Class_a a 
RIGHT OUTER JOIN Class_b b 
ON  a.id = b.id;
```

- 内连接相当于求集合的交集（intersect）/积
- 全外连接相当于求集合的并集（union）/和

## 差集
```sql
-- 求差集：A-B
SELECT  a.id AS id
        , a.name AS a_name
FROM    Class_a a 
LEFT OUTER JOIN Class_b b
ON      a.id = b.id
WHERE   b.name IS NULL;
-- B-A
SELECT  b.id AS id
        , B.name AS b_name
FROM    Class_b b
LEFT OUTER JOIN Class_a a
ON      b.id = a.id
WHERE   a.name IS NULL;
```

## 异或集
- `(A UNION B) EXCEPT (A INTERSECT B)`
- `(A EXCEPT B) UNION (B EXCEPT A)`
- 全外连接
  

```sql
-- 使用全外连接求异或集
SELECT  COALESCE(a.id, b.id) AS id
        , COALESCE(a.name, b.name) AS name
FROM    Class_a a 
FULL OUTER JOIN Class_b b
ON      a.id = b.id
WHERE   a.name IS NULL OR b.name IS NULL;
```

# 关联子查询
- 对同一行数据进行列间比较，只需在`WHERE`子句里写上比较条件
- 对不同行数据进行列间比较，需要使用关联子查询或窗口函数

## 比较过去
```sql
-- 求与上一年营业额一样的年份：
-- (1)使用关联子查询
SELECT  year
        , sale
FROM    Sales s1
WHERE   sale = (SELECT sale FROM Sales s2 WHERE s2.year = s1.year - 1)
ORDER BY    year;
-- (2)使用自连接
SELECT  s1.year
        , s1.sale
FROM    Sales s1, Sales s2
WHERE   s2.sale = s1.sale
AND     s2.year = s1.year - 1
ORDER BY    s1.year;
```

```sql
-- 比较营业额相较于上一年的变化
-- (1)使用关联子查询
SELECT  s1.year
        , s1.sale
        , CASE  WHEN sale = (SELECT sale FROM Sales s2 WHERE s2.year = s1.year - 1) THEN '→'  -- 持平
                WHEN sale > (SELECT sale FROM Sales s2 WHERE s2.year = s1.year - 1) THEN '↑' -- 增长
                WHEN sale < (SELECT sale FROM Sales s2 WHERE s2.year = s1.year - 1) THEN '↓' -- 减少
        ELSE '——' END AS var
FROM    Sales s1
ORDER BY    year;
-- (2)使用自连接
SELECT  s1.year
        , s1.sale
        , CASE  WHEN s1.sale = s2.sale THEN '→'
                WHEN s1.sale > s2.sale THEN '↑'
                WHEN s1.sale < s2.sale THEN '↓'
        ELSE '——' END AS var
FROM    Sales s1, Sales s2
WHERE   s2.year = s1.year - 1
ORDER BY    year;
-- 注意：使用自连接，最早的年份不会出现在结果里
-- 以上写法，year需连续才可比较
```


```sql
-- 若时间轴有间断
-- 与过去最邻近的年份进行比较
-- (1)使用关联子查询
SELECT  year
        , sale
FROM    Sales s1
WHERE   sale = (
    SELECT  sale
    FROM    Sales s2
    WHERE   s2.year = (
        SELECT  MAX(year)  -- 过去的年份中，年份最新的那个
        FROM    Sales s3
        WHERE   s1.year > s3.year -- 与s1.year相比是过去的年份
    )
)
ORDER BY    year;
-- (2)使用自连接：可减少一层子查询的嵌套
SELECT  s1.year
        , s1.sale
FROM    Sales s1, Sales s2
WHERE   s1.sale = s2.sale
AND     s2.year = (SELECT MAX(year) FROM Sales s3 WHERE s1.year > s3.year)
ORDER BY    year;
-- 自连接的结果里不包括最早的年份
```

```sql
-- 求每一年与过去最邻近的年份之间的营业额之差
-- 使用自外连接，结果里包含最早的年份
SELECT  s2.year AS pre_year
        , s1.year AS now_year
        , s2.sale AS pre_sale
        , s1.sale AS now_sale
        , s1.sale - s2.sale AS diff_sale
FROM    Sales s1 LEFT OUTER JOIN Sales s2
ON      s2.year = (SELECT MAX(year) FROM Sales s3 WHERE s1.year > s3.year)
ORDER BY    now_year;
```

## 累计值

```sql
-- 求截止到某个日期的营业额累计值
-- (1)使用窗口函数
SELECT  date
        , amt
        , SUM(amt) OVER(ORDER BY date) AS total_amt
FROM    Accounts;
-- (2)使用冯·诺伊曼型递归集合（标准SQL-92下）
SELECT  date
        , a1.amt
        , (SELECT SUM(amt) FROM Accounts a2 WHERE a1.date >= a2.date) AS total_amt
FROM    Accounts a1
ORDER BY    date;
```

```sql
-- 求前3行（包括本行）的累计值
SELECT  date
        , amt
        , SUM(amt) OVER(ORDER BY date ROWS 2 PRECEDING) AS total_amt
FROM    Accounts ;
-- 不满3行的时间区间也输出
SELECT  date
        , a1.amt
        , (
            SELECT  SUM(amt) FROM Accounts a2 
            WHERE   a1.date >= a2.date
            AND  
            (
                SELECT   COUNT(*) FROM Accounts a3
                    WHERE    a3.date BETWEEN a2.date AND a1.date
                ) <= 3
        ) AS mvg_sum
FROM    Accounts a1
ORDER BY date;
-- 不满3行的区间按无效处理
SELECT  date
        , a1.amt
        , (
            SELECT   SUM(amt) FROM Accounts a2 
            WHERE    a1.date >= a2.date
            AND      (
                        SELECT  COUNT(*)
                        FROM    Accounts a3
                        WHERE   a3.date BETWEEN a2.date AND a1.date
                    ) <= 3 
            HAVING  COUNT(*) = 3  -- 不满3行数据的不显示
           ) AS mvg_sum
FROM    Accounts a1
ORDER BY    date;
```

## 移动平均值
- 若要计算移动平均值，只需把上述例子中的`SUM`改为`AVG`即可

## 查询重叠的时间区间
```sql
-- 求重叠的住宿期间
SELECT  reserver
        , start_date
        , end_date
FROM    tbl t1
WHERE EXISTS (
    SELECT  * 
    FROM    tbl t2
    WHERE   t1.reserver <> t2.reserver  -- 与其他客人比较
    AND     
    (
        t1.start_date BETWEEN t2.start_date AND t2.end_date -- 自己的入住日期在他人的住宿期间内
        OR 
        t1.end_date BETWEEN t2.start_date AND t2.end_date  -- 自己的离店日期在他人的住宿期间内
    )
);
-- 如果有个人的住宿期间完全包含他人的住宿期间，则上述结果并不会输出这样的区间
-- 如果要把“完全包含他人的住宿期间的情况”也输出
SELECT  reserver
        , start_date
        , end_date
FROM    tbl t1
WHERE EXISTS
    (
        SELECT  *
        FROM    tbl t2
        WHERE   t1.reserver <> t2.reserver
        AND     
            (
                (
                    t1.start_date BETWEEN t2.start_date AND t2.end_date
                    OR
                    t1.end_date BETWEEN t2.start_date AND t2.end_date
                )
                OR 
                (
                    t2.start_date BETWEEN t1.start_date AND t1.end_date
                    AND -- 注意
                    t1.end_date BETWEEN t1.start_date AND t1.end_date
                )
            )
    );
```

# 集合运算
- 直接使用`UNION`或`INTERSECT`，结果里不会出现重复的行；若要在结果里留下重复的行，可以加上`ALL`，即`UNION ALL`
- 集合运算符为了排除掉重复行，默认地会发生排序，加上可选项`ALL`后，就不会再排序，所以性能会有提升
- 集合运算符有**优先级**：`INTERSECT`比`UNION`和`EXCEPT`优先级更高
- 集合运算支持：
  - SQL Server从2005版开始支持`INTERSECT`和`EXCEPT`；而MySQL都不支持
  - Oracle实现了`EXCEPT`功能，但命名为`MINUS`


```sql

```

```sql

```


```sql

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
[1] [Notes | SQL进阶教程 | 001](https://mp.weixin.qq.com/s?__biz=MzU5NzkxODMxOA==&mid=2247486368&idx=1&sn=3ef74be0a7b7345cba471da1370f8986&chksm=fe4d5ca1c93ad5b7481d65a8fa0a8b4e585714af57ad69437d599bbac5830910562a9097a12b&token=1415910284&lang=zh_CN#rd)

# 推荐阅读
- {% post_link sql-sql必知必会 《SQL必知必会》读书笔记%}
- {% post_link sql-sql解惑 《SQL解惑》读书笔记%}
