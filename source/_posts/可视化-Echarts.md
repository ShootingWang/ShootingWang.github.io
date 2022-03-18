---
title: 可视化 | Echarts
date: 2021-07-25 00:14:21
tags: [Echarts]
categories: Everything
mathjax: true
toc: true
hide: true
notshow: true
---

<center></center>
<!--more-->

# Echarts

# 安装方式
尝试了两种方法：
1. 安装插件`hexo-tag-echarts`
2. 下载相关js文件

> 方式一并未成功，方式二成功在博客展示动态图

## 安装插件
```md
npm install hexo-tag-echarts --save
```

并在markdown文中写：
```md
{% echarts 800 '85%' %}
// 800表示图表展示的高度为800px
// '85%'表示图表展示的宽度为85%
{
    title: {
        text: '天气情况统计',   // 主标题
        subtext: '虚构数据',    // 副标题 
        left: 'center'
    },
    tooltip: {
        trigger: 'item', 
        formatter: "{a} <br/>{b}：{c}（{d}%）"
    },
    legend: { // 图例
        // orient: 'vertical',
        // top: 'middle',
        bottom: 10,
        left: 'center', 
        data: ['西凉', '益州', '成都', '杭州']
    },
    series: [ // 序列/数据
        {
            name: '天气数据',
            type: 'pie',    //  饼图
            radius: '75%',
            center: ['50%', '50%'],
            selectedMode: 'single',
            data: [
                {value:520, name:'西凉'},
                {value:510, name:'益州'},
                {value:627, name:'成都'},
                {value:826, name:'杭州'}
            ],
            itemStyle: {
                emphasis: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            }
        }
    ]
}
{% endecharts %}
```

但并未成功在博客展示动图。

## 下载相关js文件
1. 下载`echarts.min.js`和`jquery.js`文件
    - 在[echarts.min.js下载地址](https://github.com/apache/echarts/tree/4.6.0/dist)中找到`echarts.min.js`，右键点击“将链接另存为”将文件保存到本地
    - 在[jquery官网](https://jquery.com/download/)找到“Download the uncompressed, development jQuery 3.6.0”，打开，将文本复制粘贴到js文件中，保存为“jquery.js”文件
2. 将上述两个js文件保存在`hexo\themes\<theme_name>\source\js`文件夹下
3. 在markdown文件中写入需要如下文本：

```md
<div id="test1" style="width:100%;height:500px;"></div>
<script type="text/javascript"src="/js/echarts.min.js"></script>
<script type="text/javascript"src="/js/jquery.js"></script>
<script type="text/javascript">
var myChart1 = echarts.init(document.getElementById("test1"));
option1 = {
    title: {
        text: '天气情况统计',   // 主标题
        subtext: '虚构数据',    // 副标题 
        left: 'center'
    },
    tooltip: {
        trigger: 'item', 
        formatter: "{a} <br/>{b}：{c}（{d}%）"
    },
    legend: { // 图例
        // orient: 'vertical',
        // top: 'middle',
        bottom: 10,
        left: 'center', 
        data: ['西凉', '益州', '成都', '杭州']
    },
    series: [ // 序列/数据
        {
            name: '天气数据',
            type: 'pie',    //  饼图
            radius: '75%',
            center: ['50%', '50%'],
            selectedMode: 'single',
            data: [
                {value:520, name:'西凉'},
                {value:510, name:'益州'},
                {value:627, name:'成都'},
                {value:826, name:'杭州'}
            ],
            itemStyle: {
                emphasis: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            }
        }
    ]
};
myChart1.setOption(option1);
window.onresize=function(){
    myChart1.resize();
}
</script>
```

动态饼图效果如下：
<div id="test1" style="width:100%;height:500px;"></div>
<script type="text/javascript"src="/js/echarts.min.js"></script>
<script type="text/javascript"src="/js/jquery.js"></script>
<script type="text/javascript">
var myChart1 = echarts.init(document.getElementById("test1"));
option1 = {
    title: {
        text: '天气情况统计',   // 主标题
        subtext: '虚构数据',    // 副标题 
        left: 'center'
    },
    tooltip: {
        trigger: 'item', 
        formatter: "{a} <br/>{b}：{c}（{d}%）"
    },
    legend: { // 图例
        // orient: 'vertical',
        // top: 'middle',
        bottom: 10,
        left: 'center', 
        data: ['西凉', '益州', '成都', '杭州']
    },
    series: [ // 序列/数据
        {
            name: '天气数据',
            type: 'pie',    //  饼图
            radius: '75%',
            center: ['50%', '50%'],
            selectedMode: 'single',
            data: [
                {value:520, name:'西凉'},
                {value:510, name:'益州'},
                {value:627, name:'成都'},
                {value:826, name:'杭州'}
            ],
            itemStyle: {
                emphasis: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            }
        }
    ]
};
myChart1.setOption(option1);
window.onresize=function(){
    myChart1.resize();
}
</script>

# 样例
## 饼图
{% echarts 800 '85%' %}
// 800表示图表展示的高度为800px
// '85%'表示图表展示的宽度为85%
{
    title: {
        text: '天气情况统计',   // 主标题
        subtext: '虚构数据',    // 副标题 
        left: 'center'
    },
    tooltip: {
        trigger: 'item', 
        formatter: "{a} <br/>{b}：{c}（{d}%）"
    },
    legend: { // 图例
        // orient: 'vertical',
        // top: 'middle',
        bottom: 10,
        left: 'center', 
        data: ['西凉', '益州', '成都', '杭州']
    },
    series: [ // 序列/数据
        {
            name: '天气数据',
            type: 'pie',    //  饼图
            radius: '75%',
            center: ['50%', '50%'],
            selectedMode: 'single',
            data: [
                {value:520, name:'西凉'},
                {value:510, name:'益州'},
                {value:627, name:'成都'},
                {value:826, name:'杭州'}
            ],
            itemStyle: {
                emphasis: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            }
        }
    ]
}
{% endecharts %}

## 折线图

## 柱状图








# 参考资料
- [在Hexo博客中使用ECharts来画图](https://underdream.github.io/post/writing/hexo/embed-echarts-in-hexo/)
- [如何在hexo主题里用echarts写思维导图](https://blog.csdn.net/qq_41426117/article/details/105416911)
