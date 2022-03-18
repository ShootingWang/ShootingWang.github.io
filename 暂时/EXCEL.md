---
title: EXCEL
date: 2020-06-02 10:31:07
tags: [Excel]
notshow: true
hide: true
mermaid: true
mathjax: true
math: true
---

<center></center>
<!--more-->


# 函数
## VLOOKUP
查找引用
`VLOOKUP(查找值, 查找区域, 返回查找区域第N列, 查找模式)`
- `查找模式`：
 - `0`：模糊查找
 - `1`：精确查找
- VLOOKUP函数如果查找不到对应值，会显示错误值`#N/A`

{% note default %}
excel工作簿a中有两列id、age,工作簿b中有一列id,需要找到工作薄b中id对应的age,可用的函数包括
- `index+match`
- `vlookup`

[题目来源](https://www.nowcoder.com/test/question/done?tid=35946648)
{% endnote %}

# 参考资料
- [excel 这也许是史上最好最全的VLOOKUP函数教程](https://baijiahao.baidu.com/s?id=1603886666150544094&wfr=spider&for=pc)