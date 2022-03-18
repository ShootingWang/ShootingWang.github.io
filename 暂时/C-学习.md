---
title: C++学习
date: 2020-05-08 14:08:14
tags: [C++]
categories:
hide: true
notshow: true
math: true
mathjax: true

---

<center>C plus plus</center>
<!--more-->

# C++

## getchar()
用于暂停程序，以便查看程序输出的内容

```cpp
#include <stdio.h>

int main()
{
    int a[11],i,j,t;
    for(i=0;i<=10;i++)
        a[i]=0;  //初始化为0
    
    for(i=1;i<=5;i++)
    {
        scanf("%d",&t);
        a[t]++;
    }
    
    for(i=0;i<=10;i++)
        for(j=1;j<=a[i];j++)
            printf("%d",i);
    
    getchar();getchar();  //用来暂停程序
    return 0;
}
```
