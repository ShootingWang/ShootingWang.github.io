---
title: SQL | 《SQL解惑》
date: 2020-05-25 13:06:35
tags: [SQL]
categories: [数据库]
mathjax: true
toc: true
hide: true
notshow: true
---

<center>《SQL解惑（第二版）》学习笔记</center>
<!--more-->

# 财政年度表
- 创建表格时需要添加约束，保证表格的正确性

```sql
CREATE TABLE FiscalYears
(fiscal_year INTEGER NOT NULL PRIMARY KEY,   -- 主键约束
start_date DATE NOT NULL,
CONSTRAINT valid_start_date  -- 判断start_date是否符合会计年度规定
			CHECK ( (EXTRACT(YEAR FROM start_date)  = fiscal_year - 1)  -- EXTRACT()从start_date中提取年份
				 AND (EXTRACT(MONTH FROM start_date) = 10)
                 AND (EXTRACT(DAY FROM start_date) = 01) ),
end_date DATE NOT NULL,
CONSTRAINT valid_end_date  -- 判断end_date是否符合会计年度规定
			CHECK ((EXTRACT(YEAR FROM end_date) = fiscal_year)
				AND (EXTRACT(MONTH FROM end_date) = 09)
                AND (EXTRACT(DAY FROM end_date) = 30)));
```

但是……错误的语句竟然成功插入了？？
```sql
mysql> INSERT INTO FiscalYears VALUES
    -> (1996, '1995-10-01', '1996-08-30');
Query OK, 1 row affected (0.00 sec)

mysql> SELECT * FROM FiscalYears;
+-------------+------------+------------+
| fiscal_year | start_date | end_date   |
+-------------+------------+------------+
|        1996 | 1995-10-01 | 1996-08-30 |
+-------------+------------+------------+
1 row in set (0.00 sec)
```

bug待解决……

# 缺勤者
