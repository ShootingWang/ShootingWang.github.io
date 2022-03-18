---
title: Python | jieba
date: 2020-11-05 22:44:25
tags: [Python]
categories: Python
mathjax: true
toc: true
---

<center>分词</center>
<!--more-->

# jieba
jieba支持三种分词模式：
- 精确模式：试图将句子最精确地切开，适合文本分析
- 全模式：把句子中所有的可以成词的词语都扫描出来
 - 速度非常快
 - 但是不能解决歧义
- 搜索引擎模式：在精确模式的基础上，对长词再次切分，提高召回率，适合用于搜索引擎分词

此外，jieba还支持
- 繁体分词
- 自定义词典

```python
# 安装
pip install jieba
```

## .analyse
### extract_tags()
`jieba.analyse.extract_tags(sentence, topK=20, withWeight=False, allowPOS=())`：基于TF-IDF算法的关键词抽取
- `sentence`：待提取的文本
- `topK`：返回K个TF-IDF权重最大的关键词
- `withWeight`：是否一并返回关键词权重值
- `allowPOS`：仅包括指定词性的词。默认为空，表示不筛选

### set_idf_path()
可以将关键词提取所使用逆向文本频率（IDF）文本语料库切换成自定义语料库的路径

### set_stop_words()
可以将关键词提取所使用停用词（stop words）文本语料库切换成自定义语料库的路径

### textrank()
基于TextRank算法的关键词提取

### TextRank()
新建自定义TextRank实例

### TFIDF()
新建TFIDF实例


## cut()
分词
- `cut_all`：是否采用全模式
- `HMM`：是否使用HMM模型
- 返回的结构都是一个可迭代的generator，可以使用`for`循环来获得分词后得到的每一个词语


```python
import jieba

df['cut'] = df['comment'].apply(lambda x: list(jieba.cut(x)))
## 若要将字符串中的符号去除再分词
df['cut'] = df['name'].apply(lambda x: list(jieba.cut(re.sub(r"[0-9\s+\.\!\/_,$%^*()?;；:-【】+\"\']+|[+——！，;:。？、~@#￥%……&*（）]+", "", " ".join(x)))))
```

## cut_for_search()
- `HMM`：是否使用HMM模型
- 适合用于搜索引擎构建倒排索引的分词，粒度比较细
- 返回的结构都是一个可迭代的generator，可以使用`for`循环来获得分词后得到的每一个词语

## disable_parallel()
关闭并行分词模式

## enable_parallel()
开启并行分词模式，参数为并行进程数



## lcut()
分词
- 返回结果是列表

## lcut_for_search()
- 返回结果是列表

## load_userdict()
`load_userdict(file_name)`：添加自定义词典，载入词典
- `file_name`：文件类对象或自定义词典的路径。若为路径或二进制方式打开的文件，则文件必须为UTF-8编码
- 词典格式：
 - 一个词占一行
 - 每一行分三部分：词语、词频（可省略）、词性（可省略）。用空格隔开，顺序不可颠倒



## .posseg
### dt()
默认词性标注分词器


### POSTokenizer()
`jieba.posseg.POSTokenizer(tokenizer=None)`：新建自定义分词器
- `tokenizer`可以指定为内部使用的`jieba.Tokenizer`分词器

## tokenize()
返回词语在原文的起止位置
- 输入参数只接受unicode
- `mode`：可选值有`search`（搜索模式）

```python
import jieba
result = jieba.tokenize(u'数据结构与算法')  ## 默认模式
for tk in result:
 print("word %s \t\t start: %d \t\t end: %d" %(tk[0], tk[1], tk[2]))
```



```python

```

# 参考资料
- [Python中文分词及词频统计](https://www.jianshu.com/p/7ad0cd33005e)
- [python 结巴分词(jieba)详解](https://www.cnblogs.com/jackchen-net/p/8207009.html)