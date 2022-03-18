---
title: Spark | PySpark
date: 2021-07-31 21:56:16
tags: [Data Scientist, Spark]
categories: Data Scientist
math: true
mathjax: true
---

<center></center>
<!--more-->

# 读取数据
```python
## 从HDFS读取
from pyspark.sql import SparkSession, HiveContext, DataFrameWriter
import time


spark = SparkSession.builder.enableHiveSupport().appName('test').getOrCreate()
start = time.time()

## HDFS上载入parquet格式
input = '/aaa/bbb/ccc'
data = spark.read.parquet(input)
data.show(5) ## 预览前5行
```


```python
from pyspark.sql import SparkSession, HiveContext
import pandas as pd

spark = SparkSession.builder.enableHiveSupport().appName('test').getOrCreate()
hive_context = HiveContext(spark)
sql = '''
select  start_date
        , end_date
from    db_nam.table_name
where   pt = '2021-07-30'
and     type = 'type1'
'''
df = hive_context.sql(sql)
df.show(5)   ## 预览前5行
```



```python

```

# 参考资料
- [PySpark读取和存入数据的三种方法](https://www.cnblogs.com/xiximayou/articles/13817514.html)