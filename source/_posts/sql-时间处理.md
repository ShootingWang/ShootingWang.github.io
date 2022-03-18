---
title: SQL | 时间处理
date: 2022-01-19 15:58:34
tags: [SQL]
categories: 数据库
mathjax: true
toc: true
---

<center>SQL时间相关处理~</center>
<!--more-->

# 基本语法
## 年
`YEAR()`：取日期中的年份

```sql
year('2021-12-31')
-- 2021
```

去年同一天：
```sql
add_months(current_date, -12)
```


## 月
`MONTH()`：取日期中的月份

当月第一天：
```sql
date_format(current_date, 'yyyy-MM-01')
```

当月最后一天：
```sql
last_day(current_day)
```


上个月末：
```sql
date_sub(date_format(current_date, 'yyyy-MM-01'), 1)
```

上个月同一天：
```sql
add_months(current_date, -1)
```




## 周
`WEEKOFYEAR(STRING date)`：返回时间字符出位于一年中的第几周内

```sql
weekofyear('2016-12-08 10:05:15')
-- 49
```

周几/星期几：

```sql
case    when cast(pmod(datediff(pt, '1900-01-07'), 7) as int) = 0 then 7 
        else cast(pmod(datediff(pt, '1900-01-07'), 7) 
end as weekdays
-- cast(pmod(datediff(pt, '1900-01-07'), 7)取值范围0-6
-- 或
cast(date_format(pt, 'u') as int)  -- 取值范围1-7
```

当周的周一：
```sql
date_sub(current_date, cast(date_format(current_date, 'u') as int) - 1)
```

最近一个周日：
```sql
date_sub(current_date, cast(date_format(current_date, 'u') as int))
```

## 格式转换
将`yyyy/MM/dd`格式的日期转换为`yyyy-MM-dd`格式：

```sql
concat_ws('-', split(date, '/')[0], lpad(split(date, '/')[1], 2, '0'), lpad(split(date, '/')[2], 2, '0'))
```



```sql

```


# 参考资料