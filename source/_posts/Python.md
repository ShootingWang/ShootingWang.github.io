---
title: Python
date: 2020-05-03 13:54:22
tags: [Python]
categories: Python
mathjax: true
toc: true
top: 9 
#true
index_img: /img/Python.png  ## 封面图片
---

<center>Python使用手册</center>

<!--more-->

# 模块
- {% post_link python-altair Altair %}
  - 绘图
  - 可在Jupyter环境放大、缩小、交互
- {% post_link python-categorical-encoders Categorical_encoders %}
  - 对类别变量进行0-1编码、独热编码(One-Hot Encoding)
- {% post_link python-cmath cmath %}
  - 对复数进行操作
- {% post_link python-collections模块 Collections %}
  - 对队列进行操作
- {% post_link python-dython Dython %}
    - 简易数据建模库
- {% post_link python-heapq heapq %}
  - 对堆进行操作
  - 可进行堆排序
- {% post_link python-jieba jieba %}
    - 文本分词
- {% post_link python-keras Keras %}
- {% post_link python-matplotlib matplotlib %}
    - 绘图
- {% post_link python-missingno missingno %}
    - 可视化缺失值
- {% post_link python-numpy模块 NumPy %}
    - 对数组、矩阵操作
- {% post_link python-pandas模块 Pandas %}
    - 对序列Series、数据框DataFrame进行操作
- {% post_link python-pip pip %}
  - 安装python模块
- {% post_link python-pyecharts模块 Pyecharts %}
- {% post_link python-re模块 re %}
- {% post_link python-seaborn Seaborn %}
    - 基于Matplotlib的数据可视化库
- {% post_link python-sklearn sklearn %}
- {% post_link python-string string %}
- {% post_link python-textwrap TextWrap  %}

{% post_link  %}

# 基础
- {% post_link python-命名方式 命名方式 %}
- {% post_link python-4-运算符优先级 运算符 %}
- {% post_link python-5-函数 函数 %}
- {% post_link python-6-循环 循环 %}
- {% post_link python-8-模块 模块 %}

## 对象
Python中的对象包含三个基本要素：
- id （身份标识）
- type （数据类型）
- vlaue （值）


## 切片操作
- Python的切片操作不会引起下标越界异常
```Python
lst = ['a', 'b', 'c', 'd']
print(lst[15:])
## 输出 []
```

- 索引操作可能会引起下标越界异常
```Python
lst = ['a', 'b', 'c', 'd']
print(lst[15])
#Traceback (most recent call last):
#
#  File "<ipython-input-9-bcb28d671f1f>", line #2, in <module>
#    print(lst[15])
#
#IndexError: list index out of range
```

## 命名

### xxx
公用的命名方法
- 标识符由字母-数字-下划线组成
- 不能以数字开头
- 区分大小写

### _xxx
- 半保护命名（protect）
- 代表不能直接访问的类属性
- 只有类对象和子类对象能访问
- 在模块或类外不可以使用
- 不能用`from module import * `导入
- 为了避免与子类的方法名称冲突

### __xxx
- 全私有、全保护（private）
- 类的私有成员
- 只有类对象自己能访问，而子类对象不能访问
- 不能用`from module import * `导入
- 实际上是 `_classname__xxx`

### __xxx__
- 内建方法
- 用户避免使用该定义方法



# 数据类型
Python3的6个标准数据类型：
- 不可变：
  - 数字Number
  - 字符串String
  - 元组Tuple
- 可变：
  - 列表List
  - 字典Dictionary
  - 集合Set



## 数字Number
Python3支持int、float、bool、complex数字类型。

### 复数
- 表示复数的语法是`real + image j`
- 实部`real`和虚部`image`都是浮点数
- 虚部的后缀可以是`j`或`J`
- 复数的conjugate方法可以返回该复数的共轭复数

```Python
lst1 = [1, 2, 3]  ## 可变对象
lst2 = lst1  ## 引用传递
lst1.append(5)
lst2
# [1, 2, 3, 5]
## 不可变对象为值传递
```

## 字符串
{% post_link python-字符串 %}

## 元组Tuple
- 是一种**有序**列表，和list非常相似
- 一旦初始化就不能修改
- 可以获取元素但不能赋值变成其他的元素
- 元组只包含一个元素时，要在元素后面添加逗号来消除歧义
- 元组的元素不允许删除
- 可用del删除整个元组
- 元组之间可以用 + 和 * 进行运算
- 创建tuple比list要快，存储空间比list占用更小
- tuple是不可变的，可以作为dict的key
- 成员关系操作符 in 和 not in 也可以直接用在元组上
- 用圆括号封装 （）

```Python
cmp(tuple1, tuple2)  ##比较两个元组元素
len(tuple)  ##计算元组元素个数
max(tuple)  ##返回元组中元素最大值
min(tuple)  ##返回元组中元素最小值
tuple(seq)  ##把列表seq转换为元组
```

## 列表List
{% post_link python-列表 %}

- 是一种**有序**的集合，可以随时添加和删除其中的元素
- 元素类型可以不同
- list可以嵌套（即list的元素可以是list）
- 成员关系操作符 in 和 not in 可直接用在列表上
- list 是可变的对象
- 用方括号封装 [ ]

```Python
lst.append(a)   ##在列表lst尾部添加元素a
lst.insert(num, c)  ##在列表lst的第num+1个位置（索引为num）插入元素c
lst.pop()  ##删除列表lst的最后一个元素，并返回该元素
lst.pop(i)  ##删除列表lst的索引为i的元素（i可以是负数；i=-2表示删除倒数第二个元素）
```

## 字典

## 集合set()
{% post_link python-集合 %}

```python
set(("Rank"))
#{'R', 'a', 'k', 'n'}
set("Rank")
#{'R', 'a', 'k', 'n'}
set(["Rank"])
#{'Rank'}
```

# 正则表达式
正则表达式：对字符串类型数据进行匹配、提取等操作的逻辑公式。

## 元字符
元字符（Meta Characters）：标点等特殊字符。主要有` . $ * + ? | \ ^ [ ] { } ( ) `等。

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
      <th>字符</th>
      <th>说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>.</th>
      <td>匹配除换行符（\n，\r）之外的任何单个字符。<br>若要匹配包括\n在内的任何字符，则应使用 (.\|\n)。</td>
    </tr>
    <tr>
      <th>$</th>
      <td>匹配输入字符串的结束位置。<br>如果设置了regex对象的Multiline属性，则$也匹配\n或\r之前的位置。如果$置于character class （即[$]）中，则消除了其特殊意义。<br>例如，[akm$]示匹配a、k、m或$。</td>
    </tr>
    <tr>
      <th>*</th>
      <td>匹配*前面的子表达式零次或多次。<br>等价于 {0,}。</td>
    </tr>
    <tr>
      <th>+</th>
      <td>匹配+前面的子表达式一次或多次；至少匹配一次。<br>等价于{1,}</td>
    </tr>
    <tr>
      <th>?</th>
      <td>匹配?前面的子表达式零次或一次。<br>等价于{0,1}</td>
    </tr>
    <tr>
      <th>|</th>
      <td>表示可选项（或），即 | 前后的表达式任选一个。</td>
    </tr>
    <tr>
      <th>\</th>
      <td>将一个字符标记为一个特殊字符/原义字符，或一个向后引用、或一个八进制转义符。<br>例如：\n表示换行符，而 n 表示字母“n”； \ 表示右斜杠“\”；\t表示制表符。</td>
    </tr>
    <tr>
      <th>^</th>
      <td>取非匹配</td>
    </tr>
    <tr>
      <th>[]</th>
      <td>选择方括号例的任意一个。<br>例如，[0-2]与[012]完全等价，[Rr] 表示匹配字母R和r</td>
    </tr>
    <tr>
      <th>{}</th>
      <td>前面的字符或表达式的重复次数。<br>例如，{5,12}表示重复的次数不能小于5，不能多于12，否则都不匹配</td>
    </tr>
    <tr>
      <th>()</th>
      <td>提取匹配的字符串。<br>例如，(\\s*)表示连续空格的字符串</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

## 数量词
数量词（Quantifiers）

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
      <th>字符</th>
      <th>说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>{n}</th>
      <td>至少匹配n次。<br>例如，“o{2,}”不能匹配“Bob”中的“o”，但能匹配“fooooood”中的所有“o”；“o{1,}”等价于“o+”；“o{0,}”等价于“o*”</td>
    </tr>
    <tr>
      <th>{n,m}</th>
      <td>n小于等于m。最少匹配n次且最多匹配m次。<br>【注】在逗号和两个数之间不能有空格。<br>例如，“o{1,3}”表示匹配“fooooood”中的前三个“o”；“o{0,1}”等价于“o?”，匹配“o”零次或1次。</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

## 序列
序列（Sequences）：用于匹配字符序列。

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
      <th>字符</th>
      <th>说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>\b</th>
      <td>匹配一个单词边界（boundary），即单词和空格间的位置。<br>例如，“er\b”可以匹配单词“never”中的“er”，但不能匹配单词“verb”中的“er”。</td>
    </tr>
    <tr>
      <th>\B</th>
      <td>匹配非单词边界。<br>例如，“er\B”可以匹配单词“verb”中的“er”，但不能匹配单词“never”中的“er”。</td>
    </tr>
    <tr>
      <th>\d</th>
      <td>匹配任何一个数字字符（digits）。<br>等价于 “[0-9]”。</td>
    </tr>
    <tr>
      <th>\D</th>
      <td>匹配任何一个非数字字符。<br>等价于 “[^0-9]”。</td>
    </tr>
    <tr>
      <th>\f</th>
      <td>换页符。<br>等价于 “\x0c” 和 “\cL”。</td>
    </tr>
    <tr>
      <th>\h</th>
      <td>匹配水平间隔</td>
    </tr>
    <tr>
      <th>\H</th>
      <td>匹配非水平间隔</td>
    </tr>
    <tr>
      <th>\n</th>
      <td>换行符<br>等价于 “\x0a” 和 “\cJ”</td>
    </tr>
    <tr>
      <th>\r</th>
      <td>回车符<br>等价于 “\x0d” 和 “\cM”</td>
    </tr>
    <tr>
      <th>\s</th>
      <td>匹配任何空白字符，包括空格、制表符、换页符等等。<br>等价于 “[\f\n\r\t\v]”。</td>
    </tr>
    <tr>
      <th>\S</th>
      <td>匹配任何非空白字符<br>等价于 “[^\f\n\r\t\v]”。</td>
    </tr>
    <tr>
      <th>\t</th>
      <td>制表符（tab）<br>等价于 “\x09” 和 “\cl”。</td>
    </tr>
    <tr>
      <th>\v</th>
      <td>垂直制表符<br>等价于 “x0b” 和 “cK”</td>
    </tr>
    <tr>
      <th>\V</th>
      <td>匹配非垂直间隔</td>
    </tr>
    <tr>
      <th>\w</th>
      <td>匹配任何字母、数字、下划线（单词）（word）<br>等价于 “[A-Za-z0-9]”</td>
    </tr>
    <tr>
      <th>\W</th>
      <td>匹配任何非字母、数字、下划线<br>等价于“[^A-Za-z0-9]”
</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

# 运算符
## 身份运算符 is
`is`：身份运算符，比较变量的 id 地址是否相同，即是否指向同一块内存地址

## 比较运算符
`==``:比较运算符，比较变量的 value 是否相同，即不管是否是同一块内存地址，只要其值相同即可

```Python
## 涉及Python的深浅拷贝问题
##数字、字符串等不可变数据类型
##value一样，id一样
a = 2
b = 2

print(id(a), id(b))
140710848664416 140710848664416

print(a == b, a is b)
True True

##对于列表和字典等可变数据类型
##即使value一样，id也不一样
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a), id(b))
2348645341512 2348645363144

print(a == b, a is b
True False
```

```Python
## 涉及Python的深浅拷贝问题
##数字、字符串等不可变数据类型
##value一样，id一样
a = 2
b = 2

print(id(a), id(b))
140710848664416 140710848664416

print(a == b, a is b)
True True

##对于列表和字典等可变数据类型
##即使value一样，id也不一样
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a), id(b))
2348645341512 2348645363144

print(a == b, a is b)
True False
```

# 内置函数
## open()
打开一个文件，创建一个file对象

```Python
open(name[, mode[, buffering]])
```
- `name`：文件名称
- `mode`：打开文件的模式
- `buffering`：是否寄存
 - `buffering=0`：不寄存
 - `buffering=1`：访问文件时会寄存行
 - `buffering=c`：$c>1$，表明寄存区的缓冲大小
 - `buffering=c`：$c<0$，寄存区的缓冲大小为系统默认

|模式|打开方式|其他说明|
|:--:|:----|:----|
|`r`|只读|（1）文件的指针放在文件的开头<br>（2）默认模式|
|`rb`|以二进制格式打开文件用于只读|（1）文件的指针放在文件的开头<br>（2）默认模式|
|`r+`|用于读写|文件的指针放在文件的开头|
|`rb+`|以二进制格式打开文件用于读写|文件的指针放在文件的开头|
|`w`|只用于写入||
|`wb`|||
|`w+`|用于读写||
|`wb+`|以二进制格式打开文件用于读写||
|`a`|用于追加||
|`ab`|以二进制格式打开文件用于追加||
|`a+`|用于读写||
|`ab+`|以二进制格式打开文件用于追加|（1）如果该文件已存在，文件指针将会放在文件的结尾<br>（2）如果该文件不存在，创建新文件用于读写。|

## r
表示 `''`内的字符串默认不转义
```Python
print('Hello,\nWorld')  ## ''内的\n发生了转义（换行）
#Hello,
#World

print(r'Hello,\nWorld')   ## ''内的\n视为字符串（不转义）
#Hello,\nWorld
```


## str()


## zip()
```python
for part in zip(*[iter('AABCAAADA')]*3):
    print(part)
#('A', 'A', 'B')
#('C', 'A', 'A')
#('A', 'D', 'A')
```

```python
for part in zip(*[iter('AABCAAADA')]*3):
    ans = []
    for x in part:
        if x not in ans:
            ans.append(x)
    print("".join(ans))
#AB
#CA
#AD

```

# 内置数据集
## seaborn

> {% post_link python-seaborn Seaborn %}

使用`load_dataset()`载入数据集（需要联网）；seaborn含有的数据集有：
- `anscombe`
- `attention`
- `brain_networks`
- `car_crashes`
- `diamonds`
- `dots`
- `exercise`
- `flights`
- `fmri`
- `gammas`
- `iris`
- `mpg`
- `planets`
- `tips`
- `titanic`

```python
import seaborn as sns
df = sns.load_dataset('titanic')  ##载入数据集Titanic

df.sample(5)      ## 随机抽样5行
print(df.info())  ## 数据集基本信息
print(df.describe())    ## 数据集的变量的基本信息

## 离散变量的基本信息
for name in df.dtypes[(df.dtypes == "category") | (df.dtypes == "object")].index:
    print("{} 特征值 :  {}".format(name, str(df[name].unique())))
```

## sklearn

> {% post_link python-sklearn Sklearn %}

使用`load_数据集名称()`载入数据集

|函数名|详情|用途|
|:----|:-----|:----|
|`load_boston`|[Boston房价数据](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_boston.html#sklearn.datasets.load_boston)|回归|
|`load_iris`|经典的鸢尾花数据|分类|
|`load_diabetes`|糖尿病患者数据|回归|
|`load_digits`|手写数字数据|多分类|
|`load_linnerud`|||
|`load_wine`||分类|
|`load_breast_cancer`||分类|

> sklearn中较大型数据集（需要联网）请看：{% post_link python-sklearn Sklearn %}中的`datasets`


# Python2 vs Python 3

## 默认编码

| |Python 2|Python 3|
|:----:|:----:|:----:|
|默认编码|ASCII|utf-8|

## 用户交互

| |Python 2|Python 3|
|:----:|:----:|:----:|
|用户输入函数|`raw_input()`|`input()`|

## Unicode

| |Python 2|Python 3|
|:----:|:----:|:----:|
|Unicode默认|2个字节表示一个字符|4个字节表示一个字符|

## init文件

| |Python 2|Python 3|
|:----:|:----:|:----:|
|init文件|新建的包如果没有init文件，不能被调用|新建的包里面的init文件如果被删除，包照样可以被调用|


## range

| |Python 2|Python 3|
|:----:|:----:|:----:|
|`xrange`|有<br>返回一个迭代器<br>在每次循环中生成序列的下一个数字|无|
|`range`|有<br>返回一个list|有<br>相当于Python2中的`xrange`|

## print函数
| |Python 2|Python 3|
|:----:|:----:|:----:|
|`print`|是statement，不需要加括号|是函数，需要加括号|

## 整数相除
| |Python 2|Python 3|
|:----:|:----:|:----:|
|`/`|`3/2`是`int`|`3/2`是`float`|

## 异常处理

| |Python 2|Python 3|
|:----:|:----:|:----:|
|`as`关键字|不用使用|必须使用|
```Python
## Python 2
try:
    1/0
except ZeroDivisionError, e:
    print str(e)

## Python 3
try:
    1/0
except ZeroDivisionError as e:
    print(str(e))
```

## map函数

| |Python 2|Python 3|
|:----:|:----:|:----:|
|`map()`|返回list|返回迭代器（iterator）|

## 不等运算符
| |Python 2|Python 3|
|:----:|:----:|:----:|
|不等运算符|`<>` 或 `!=`|只有`!=`|


## 数据类型
| |Python 2|Python 3|
|:----:|:----:|:----:|
|数据类型|有 `long`类型|没有`long`类型，只有`int`|

## has_key方法
| |Python 2|Python 3|
|:----:|:----:|:----:|
|字典|字典支持has_key方法|字典不支持has_key方法|

```Python
person = {"age": 35, "name": "Wang Wu"}
## Python 2
print "person has key \"age\":", person.has_key("age")
# person has key "age": True
print "person has key \"age\":", "age" in person
# person has key "age": True

## Python 3
print("person has key \"age\":", "age" in person)
# person has key "age": True
```



# 其他
## 位
- bit，比特位，b
- 是<u>计算机内部数据存储的最小单位</u>，表示二进制数（也就是0, 1）

## 字节
- byte，B
- 是<u>计算机中数据处理的基本单位</u>

|简写|英文名称|中文名称|换算|
|:-:|:--:|:--|:-----|
|B|byte|字节|1B = 8 b|
|KB|Kilobyte|千字节|1KB = 1024 B|
|MB|Megabyte|兆字节/百万字节，简称兆|1 MB = 1024 KB|
|GB|Gigabyte|吉字节/十亿字节，简称千兆|1 GB = 1024 MB|
|TB|Terabyte|太字节/万亿字节|1 TB = 1024 GB|
|PB|Petabyte|拍字节/千万亿字节|1 PB = 1024 TB|
|EB|Exabyte|艾字节/百亿亿字节|1 EB = 1024 PB|
|ZB|Zettabyte|泽字节/十万亿亿字节|1 ZB = 1024 EB|


```Python

```




# 参考资料
- [Python open() 函数](https://www.runoob.com/python/python-func-open.html)
- [Python endswith() 函数](https://www.cnblogs.com/Allen-rg/p/10501125.html)
- [Python标识符](https://www.runoob.com/python/python-basic-syntax.html)
