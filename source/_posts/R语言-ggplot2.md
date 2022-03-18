---
title: R语言 | ggplot2
date: 2021-03-07 23:05:41
tags: [R,可视化]
categories: R语言
mathjax: true
toc: true
---

<center>绘图</center>
<!--more-->

# ggplot2
可绘制：
- [面积图](#geom_area)
- [条形图](#geom_bar)
- [箱线图](#geom_boxplot)
- [边际地毯](##geom_rug)


## expand_limits()
扩展坐标轴范围

```r
head(BOD)
# A data.frame: 6 × 2
#   Time	demand
#   <dbl>	<dbl>
# 1	1	8.3
# 2	2	10.3
# 3	3	19.0
# 4	4	16.0
# 5	5	15.6
# 6	7	19.8
ggplot(BOD, aes(x = Time, y = demand)) + 
    geom_line()  ## 默认y轴范围
```

<meta name="referrer" content="no-referrer" />
{% asset_img 默认y轴范围.png 默认y轴范围 %}

```r
ggplot(BOD, aes(x = Time, y = demand)) + 
    geom_line() + 
    expand_limits(y = 0)  ## 扩展y轴范围
```

<meta name="referrer" content="no-referrer" />
{% asset_img 扩展y轴范围.png 扩展y轴范围 %}


## facet_grid()

### 分面条形图

```r
library(gcookbook)
head(gcookbook::tophitters2001)
# A data.frame: 6 × 26
# id	first	last	name	year	stint	team	lg	g	ab	...	sb	cs	bb	so	ibb	hbp	sh	sf	gidp	avg
# <fct>	<chr>	<chr>	<chr>	<int>	<int>	<fct>	<fct>	<int>	<int>	...	<int>	<int>	<int>	<int>	<int>	<int>	<int>	<int>	<int>	<dbl>
# 1	walkela01	Larry	Walker	Larry Walker	2001	1	COL	NL	142	497	...	14	5	82	103	6	14	0	8	9	0.3501
# 2	suzukic01	Ichiro	Suzuki	Ichiro Suzuki	2001	1	SEA	AL	157	692	...	56	14	30	53	10	8	4	4	3	0.3497
# 3	giambja01	Jason	Giambi	Jason Giambi	2001	1	OAK	AL	154	520	...	2	0	129	83	24	13	0	9	17	0.3423
# 4	alomaro01	Roberto	Alomar	Roberto Alomar	2001	1	CLE	AL	157	575	...	30	6	80	71	5	4	9	9	9	0.3357
# 5	heltoto01	Todd	Helton	Todd Helton	2001	1	COL	NL	159	587	...	7	5	98	104	15	5	1	5	14	0.3356
# 6	aloumo01	Moises	Alou	Moises Alou	2001	1	HOU	NL	136	513	...	5	1	57	57	14	3	0	8	18	0.3314
tophit = tophitters2001[(tophitters2001$team == 'HOU') | (tophitters2001$team == 'SEA'),]
head(tophit)
# A data.frame: 6 × 26
# id	first	last	name	year	stint	team	lg	g	ab	...	sb	cs	bb	so	ibb	hbp	sh	sf	gidp	avg
# <fct>	<chr>	<chr>	<chr>	<int>	<int>	<fct>	<fct>	<int>	<int>	...	<int>	<int>	<int>	<int>	<int>	<int>	<int>	<int>	<int>	<dbl>
# 2	suzukic01	Ichiro	Suzuki	Ichiro Suzuki	2001	1	SEA	AL	157	692	...	56	14	30	53	10	8	4	4	3	0.3497
# 6	aloumo01	Moises	Alou	Moises Alou	2001	1	HOU	NL	136	513	...	5	1	57	57	14	3	0	8	18	0.3314
# 7	berkmla01	Lance	Berkman	Lance Berkman	2001	1	HOU	NL	156	577	...	7	9	92	121	5	13	0	6	8	0.3310
# 8	boonebr01	Bret	Boone	Bret Boone	2001	1	SEA	AL	158	623	...	5	5	40	110	5	9	5	13	11	0.3307
# 32	martied01	Edgar	Martinez	Edgar Martinez	2001	1	SEA	AL	132	470	...	4	1	93	90	9	9	0	9	11	0.3064
# 41	olerujo01	John	Olerud	John Olerud	2001	1	SEA	AL	159	572	...	3	1	94	70	19	5	1	7	21	0.3024
ggplot(tophit, aes(x = avg, y = name)) + 
    geom_segment(aes(yend = name), xend = 0, colour = 'grey50') + 
    geom_point(size = 3, aes(colour = lg)) + 
    scale_color_brewer(palette = 'Set1', limits = c('NL', 'AL'), guide = FALSE) + 
    theme_bw() + 
    theme(panel.grid.major.y = element_blank()) + 
    facet_grid(lg~., scales = 'free_y', space = 'free_y')
```

<meta name="referrer" content="no-referrer" />
{% asset_img 分面条形图.png 分面条形图 %}


### 分面直方图


```r
library(MASS)
head(birthwt)
# A data.frame: 6 × 10
# low	age	lwt	race	smoke	ptl	ht	ui	ftv	bwt
# <int>	<int>	<int>	<int>	<int>	<int>	<int>	<int>	<int>	<int>
# 85	0	19	182	2	0	0	0	1	0	2523
# 86	0	33	155	3	0	0	0	0	3	2551
# 87	0	20	105	1	1	0	0	0	1	2557
# 88	0	21	108	1	1	0	0	1	2	2594
# 89	0	18	107	1	1	0	0	1	0	2600
# 91	0	21	124	3	0	0	0	0	0	2622
ggplot(birthwt, aes(x = bwt)) + 
    geom_histogram(fill = 'white', colour = 'black') + 
    facet_grid(smoke ~.)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 分面条形图.png 分面条形图 %}

```r
## 修改分面标签
birthwt1 = birthwt
birthwt1$smoke = factor(birthwt1$smoke)
levels(birthwt1$smoke)
# '0''1'
library(plyr)
birthwt1$smoke = revalue(birthwt1$smoke, c('0' = 'No Smoke', '1' = 'Smoke'))
ggplot(birthwt1, aes(x = bwt)) + 
    geom_histogram(fill = 'white', colour = 'black') + 
    facet_grid(smoke~.)

```

<meta name="referrer" content="no-referrer" />
{% asset_img 分面条形图2.png 分面条形图 %}

## geom_area()
绘制**面积图**
- 百分比堆积面积图
- 堆积面积图
- 面积图

- `alpha = 0.2`：将面积图的透明度设置为80%
- `fill`：面积图填充颜色
- `colour`：边框线颜色；若不设置，则无边框线

### 百分比堆积面积图
```r
library(ggplot2)   ## 用于绘图
library(gcookbook) ## 使用当中的数据
library(plyr)

head(gcookbook::uspopage)
#   Year	AgeGroup	Thousands
#   <int>	<fct>	<int>
# 1	1900	<5   	9181
# 2	1900	5-14	16966
# 3	1900	15-24	14951
# 4	1900	25-34	12161
# 5	1900	35-44	9273
# 6	1900	45-54	6437

uspopage_prop = ddply(uspopage, "Year", transform, Percent = Thousands / sum(Thousands) * 100 )

head(uspopage_prop)
# 	Year	AgeGroup	Thousands	Percent
# <int>	<fct>	<int>	<dbl>
# 1	1900	<5   	9181	12.065340
# 2	1900	5-14	16966	22.296107
# 3	1900	15-24	14951	19.648067
# 4	1900	25-34	12161	15.981549
# 5	1900	35-44	9273	12.186243
# 6	1900	45-54	6437	8.459274

p  = ggplot(uspopage_prop, aes(x = Year, y = Percent, fill = AgeGroup)) + 
        geom_area(colour = 'black', size = .2, alpha = .4) + 
        scale_fill_brewer(palette = "Blues", breaks = rev(levels(uspopage$AgeGroup)))
p 
```

<meta name="referrer" content="no-referrer" />
{% asset_img 百分比堆积面积图.png 百分比堆积面积图 %}


### 堆积面积图
```r
library(gcookbook)

p = ggplot(uspopage, aes(x = Year, y = Thousands, fill = AgeGroup)) + 
        geom_area()
p 
```

<meta name="referrer" content="no-referrer" />
{% asset_img 堆积面积图.png 堆积面积图 %}


```r
library(plyr)
library(gcookbook)
library(ggplot2)

p = ggplot(uspopage, aes(x = Year, y = Thousands, fill = AgeGroup, order = desc(AgeGroup))) + 
        geom_area(colour = 'black', size = .2, alpha = .4) + 
        scale_fill_brewer(palette = 'Blues')
p 
## aes中order确定图例的顺序
```

<meta name="referrer" content="no-referrer" />
{% asset_img 堆积面积图2.png 堆积面积图 %}


### 面积图
- 默认填充颜色为黑色

```r
sunspotyear = data.frame(Year = as.numeric(time(sunspot.year)),
                        Sunspots = as.numeric(sunspot.year))
ggplot(sunspotyear, aes(x = Year, y = Sunspots)) + geom_area()
```

<meta name="referrer" content="no-referrer" />
{% asset_img 面积图.png 面积图 %}


```r
sunspotyear = data.frame(Year = as.numeric(time(sunspot.year)),
                        Sunspots = as.numeric(sunspot.year))
ggplot(sunspotyear, aes(x = Year, y = Sunspots)) + 
    geom_area(colour = 'black', fill = 'blue', alpha = .2)
## 填充颜色为蓝色
```

<meta name="referrer" content="no-referrer" />
{% asset_img 面积图2.png 面积图 %}


```r
ggplot(sunspotyear, aes(x = Year, y = Sunspots)) + 
    geom_area(fill = 'blue', alpha = .2) + ## 先绘制不带边框线的面积图
    geom_line() ## 再绘制轨迹
```

<meta name="referrer" content="no-referrer" />
{% asset_img 面积图3.png 面积图 %}


## geom_bar()
绘制**条形图**

- 在`aes()`中将合适的变量映射到`fill`上，对条形设置着色
- 在`scale_fill_brewer()`或`scale_fill_manual()`中可重新设定图形颜色
- `width`：设置条形宽度，默认值为0.9
- `colour = 'black'`：为条形添加黑色边框线



### 条形图

```r
head(BOD)
# A data.frame: 6 × 2
#   Time	demand
#   <dbl>	<dbl>
# 1	1	8.3
# 2	2	10.3
# 3	3	19.0
# 4	4	16.0
# 5	5	15.6
# 6	7	19.8
ggplot(BOD, aes(x = Time, y = demand)) + 
    geom_bar(stat = 'identity')
```

<meta name="referrer" content="no-referrer" />
{% asset_img 条形图3.png 条形图 %}


```r
## 调整条形宽度
head(pg_mean)
# A data.frame: 3 × 2
#  group	weight
#  <fct>	<dbl>
# 1	ctrl	5.032
# 2	trt1	4.661
# 3	trt2	5.526
ggplot(pg_mean, aes(x = group, y = weight)) + 
    geom_bar(stat = 'identity', width = 0.5)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 条形图4.png 条形图 %}


```r
library(gcookbook)
head(uspopchange)
# A data.frame: 6 × 4
#   State	    Abb	    Region	Change
#   <chr>	    <chr>	<fct>	<dbl>
# 1	Alabama	    AL	    South	7.5
# 2	Alaska	    AK	    West	13.3
# 3	Arizona	    AZ	    West	24.6
# 4	Arkansas	AR	    South	9.1
# 5	California	CA	    West	10.0
# 6	Colorado	CO	    West	16.9
upc = subset(uspopchange, rank(Change) > 40)
head(upc)
# A data.frame: 6 × 4
#       State	    Abb	    Region	Change
#       <chr>	    <chr>	<fct>	<dbl>
# 3	    Arizona	    AZ	    West	24.6
# 6	    Colorado	CO	    West	16.9
# 10	Florida	    FL	    South	17.6
# 11	Georgia	    GA	    South	18.3
# 13	Idaho	    ID	    West	21.1
# 29	Nevada	    NV	    West	35.1
ggplot(upc, aes(x = Abb, y = Change, fill = Region)) +
    geom_bar(stat = 'identity')
```

<meta name="referrer" content="no-referrer" />
{% asset_img 条形图.png 条形图 %}


```r
## 重新设定颜色
## 并根据人口变动百分比排序
ggplot(upc, aes(x = reorder(Abb, Change), y = Change, fill = Region)) + 
    geom_bar(stat = 'identity', colour = 'black') + 
    scale_fill_manual(values = c('#669933', '#FFCC66')) + 
    xlab('State')
```

<meta name="referrer" content="no-referrer" />
{% asset_img 条形图2.png 条形图 %}



### 簇状条形图
- 在x轴的分类变量上添加另一个分类变量一起对数据进行分组，将新增的分类变量映射给`fill`参数来绘制簇状条形图
- 需令参数`position = 'dodge'`，使得两组条形在水平方向上错开排列


```r
library(gcookbook)
head(cabbage_exp)
# A data.frame: 6 × 6
# Cultivar	Date	Weight	sd	n	se
# <fct>	<fct>	<dbl>	<dbl>	<int>	<dbl>
# 1	c39	d16	3.18	0.9566144	10	0.30250803
# 2	c39	d20	2.80	0.2788867	10	0.08819171
# 3	c39	d21	2.74	0.9834181	10	0.31098410
# 4	c52	d16	2.26	0.4452215	10	0.14079141
# 5	c52	d20	3.11	0.7908505	10	0.25008887
# 6	c52	d21	1.47	0.2110819	10	0.06674995
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) + 
    geom_bar(position = 'dodge', stat = 'identity')

```

<meta name="referrer" content="no-referrer" />
{% asset_img 簇状条形图.png 簇状条形图 %}


```r
library(RColorBrewer)
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) + 
    geom_bar(position = 'dodge', stat = 'identity', colour = 'black') + 
    scale_fill_brewer(palette = 'Pastell')
## colour = 'black' 添加黑色边框线
## scale_fill_brewer() 修改条形填充颜色
```

<meta name="referrer" content="no-referrer" />
{% asset_img 簇状条形图2.png 簇状条形图 %}


### 堆积条形图

```r
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) + 
    geom_bar(stat = 'identity')
```

<meta name="referrer" content="no-referrer" />
{% asset_img 堆积条形图.png 堆积条形图 %}

```r
ggplot(cabbage_exp, aes(x = Date, y = Weight, fill = Cultivar)) + 
    geom_bar(stat = 'identity') + 
    guides(fill = guide_legend(reverse = TRUE))
## 调整图例顺序
```

<meta name="referrer" content="no-referrer" />
{% asset_img 堆积条形图2.png 堆积条形图 %}



### 百分比堆积条形图

```r
library(gcookbook)
head(cabbage_exp)
# A data.frame: 6 × 6
#   Cultivar	Date	Weight	sd	n	se
#   <fct>	<fct>	<dbl>	<dbl>	<int>	<dbl>
# 1	c39	d16	3.18	0.9566144	10	0.30250803
# 2	c39	d20	2.80	0.2788867	10	0.08819171
# 3	c39	d21	2.74	0.9834181	10	0.31098410
# 4	c52	d16	2.26	0.4452215	10	0.14079141
# 5	c52	d20	3.11	0.7908505	10	0.25008887
# 6	c52	d21	1.47	0.2110819	10	0.06674995
library(plyr)
ce = ddply(cabbage_exp, 'Date', transform, 
          percent_weight = Weight / sum(Weight) * 100)
## ddply函数根据指定的变量Date对数据框cabbage_exp进行分组
## 并对各组数据执行transform()函数
head(ce)
# A data.frame: 6 × 7
# Cultivar	Date	Weight	sd	n	se	percent_weight
# <fct>	<fct>	<dbl>	<dbl>	<int>	<dbl>	<dbl>
# 1	c39	d16	3.18	0.9566144	10	0.30250803	58.45588
# 2	c52	d16	2.26	0.4452215	10	0.14079141	41.54412
# 3	c39	d20	2.80	0.2788867	10	0.08819171	47.37733
# 4	c52	d20	3.11	0.7908505	10	0.25008887	52.62267
# 5	c39	d21	2.74	0.9834181	10	0.31098410	65.08314
# 6	c52	d21	1.47	0.2110819	10	0.06674995	34.91686
ggplot(ce, aes(x = Date, y = percent_weight, fill = Cultivar)) + 
    geom_bar(stat = 'identity')
```

<meta name="referrer" content="no-referrer" />
{% asset_img 百分比堆积条形图.png 百分比堆积条形图 %}


### 频数条形图


### 正负条形图







## geom_boxplot()
绘制**箱线图**

```r
head(ToothGrowth)
# A data.frame: 6 × 3
#   len	    supp	dose
#   <dbl>	<fct>	<dbl>
# 1	4.2	    VC	0.5
# 2	11.5	VC	0.5
# 3	7.3	    VC	0.5
# 4	5.8	    VC	0.5
# 5	6.4	    VC	0.5
# 6	10.0	VC	0.5

library(ggplot2)

ggplot(ToothGrowth, aes(x = supp, y = len)) + geom_boxplot()
```

<meta name="referrer" content="no-referrer" />
{% asset_img 箱线图.png 箱线图 %}

- 使用`interaction()`可以聚合多个分组变量，用于绘制多分组变量的箱线图

```r
ggplot(ToothGrowth, aes(x = interaction(supp, dose), y = len)) + 
    geom_boxplot()
```

<meta name="referrer" content="no-referrer" />
{% asset_img 多分组变量箱线图.png 多分组变量箱线图 %}

```r
## 分组箱线图
ggplot(ChickWeight, aes(x = Time, y = weight)) + 
    geom_boxplot(aes(group = Time))
```

<meta name="referrer" content="no-referrer" />
{% asset_img 分组变量箱线图.png 分组变量箱线图 %}

```r

```


```r

```



## geom_density()
绘制**密度曲线图**



## geom_histogram()



## geom_line()



## geom_point()
绘制**散点图**


## geom_ribbon()
- 为折线图添加置信域


## geom_rug()
- 向图添加**边际地毯**（Marginal rugs）

```r
head(faithful)
# A data.frame: 6 × 2
#   eruptions	waiting
#   <dbl>	<dbl>
# 1	3.600	79
# 2	1.800	54
# 3	3.333	74
# 4	2.283	62
# 5	4.533	85
# 6	2.883	55
library(ggplot2)

ggplot(faithful, aes(x = eruptions, y = waiting)) + 
    geom_point() + geom_rug()

```

<meta name="referrer" content="no-referrer" />
{% asset_img 点图添加边际地毯.png 点图添加边际地毯 %}


- 通过向边际地毯线的位置坐标添加扰动`position`并设定`size`减小线宽可以减轻边际地毯线的重叠

```r
ggplot(faithful, aes(x = eruptions, y = waiting)) + 
    geom_point() + 
    geom_rug(position = 'jitter', size = .2)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 点图添加边际地毯2.png 点图添加边际地毯 %}



## stat_function()
绘制**函数图像**

```r
myfun = function(xvar){
    1 / (1 + exp(-xvar + 10))
}

library(ggplot2)
ggplot(data.frame(x = c(0, 20)), aes(x = x)) + 
    stat_function(fun = myfun, geom = 'line')
```

<meta name="referrer" content="no-referrer" />
{% asset_img 函数图像.png 函数图像 %}


```r

```




```r

```


# 参考资料
- []()
- []()