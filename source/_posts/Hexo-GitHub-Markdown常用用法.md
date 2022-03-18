---
title: Hexo+GitHub+Markdown常用用法
date: 2020-04-28 14:31:53
tags: [Hexo,GitHub,Markdown]
categories: Everything
index_img: /img/hexo.png
mathjax: true
math: true
toc: true
highlight: ##代码高亮
  enable: true
#  style: 'Vs 2015'
  ## 样式选择：https://highlightjs.org/static/demo/
  copy_btn: true ##是否开启复制代码的按钮
##top: true ## 置顶
---

<center>记录一些Hexo+Markdown的常用语法~</center>

<!--more-->

<!--toc-->



# Hexo
## 部署
### 新建博客
```shell
$ hexo n '博客名称'
```
### 上传
```shell
$ hexo g
```
即 `hexo generate`

## npm
- 安装、卸载插件

```md
// 安装插件
npm install 插件名称

// 卸载插件
npm uninstall 插件名称
```

npm升级：
```md
npm install -g npm
```

## 标题自动编号
- [为Hexo博客标题自动添加序号：hexo-heading-index](http://r12f.com/posts/adding-index-to-your-headings-with-hexo-heading-index/)

## 代码高亮
- [使用prism.js进行代码高亮](https://www.zfl9.com/hexo-code.html)
- [markdown代码块支持的语言](https://www.jianshu.com/p/1f223eb78ad8)

## Fluid主题
安装可见：[Hexo-fluid](https://hexo.fluid-dev.com/docs/guide/#%E4%B8%BB%E9%A2%98%E7%AE%80%E4%BB%8B)

压缩包[下载](https://github.com/fluid-dev/hexo-theme-fluid/releases/tag/v1.8.0-beta2)


## Fork me on GitHub
- [在右上角实现fork me on github](https://blog.csdn.net/fly_wt/article/details/86674138)


## 流程图
- [Hexo引入Mermaid流程图和MathJax数学公式](https://www.cnblogs.com/icoty23/p/10911231.html)
- [Hexo中插入mermaid diagrams](https://blog.csdn.net/Olivia_Vang/article/details/92987859#%E8%AF%AD%E6%B3%95)
- [Hexo中引入Mermaid流程图](https://tyloafer.github.io/posts/7790/)
- [hexo集成mermaid画图](https://rogersnowing.cn/post/38b5106c.html)
- [Markdown里面使用mermaid画流程图（基础）](https://blog.csdn.net/Subson/article/details/78054689)

## NeXT主题
- [Hexo+NexT 打造一个炫酷博客](https://juejin.im/post/5bcd2d395188255c3b7dc1db#heading-11)
- [Hexo-NexT 主题样式美化 - 动画设置](https://tding.top/archives/dfac1e9c.html)
- [利用 Hexo+Next 搭建个人博客（三）—— 优化 (各种 tips、黑科技，不断更新)](https://lvxuefei.top/%E5%88%A9%E7%94%A8Hexo-Next%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%EF%BC%88%E4%B8%89%EF%BC%89-%E4%BC%98%E5%8C%96-%E5%90%84%E7%A7%8Dtips%E3%80%81%E9%BB%91%E7%A7%91%E6%8A%80%EF%BC%8C%E4%B8%8D%E6%96%AD%E6%9B%B4%E6%96%B0/)
- [next 主题背景添加 canvas nest 特效](https://jzwdsb.github.io/2019/02/next_canvax/)
- [Hexo Next阅读次数不正常、显示多个阅读次数](https://blog.qust.cc/archives/63320.html)
- [打造个性超赞博客 Hexo + NexT + GitHub Pages 的超深度优化](https://io-oi.me/tech/hexo-next-optimization/)


## 评论
- [hexo-fluid添加utterances评论功能](https://litstronger.github.io/2020/04/03/hexo-fluid%E6%B7%BB%E5%8A%A0utterances%E8%AF%84%E8%AE%BA%E5%8A%9F%E8%83%BD/)
- [Hexo-NexT 配置 Valine](https://tding.top/archives/ed8b904f.html)
- [Valine 评论使用报错 504](https://zucchiniy.cn/archives/c7b31ff.html)
- [Hexo博客进阶：为Next主题添加Valine评论系统](https://qianfanguojin.github.io/2019/07/23/Hexo%E5%8D%9A%E5%AE%A2%E8%BF%9B%E9%98%B6%EF%BC%9A%E4%B8%BANext%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0Valine%E8%AF%84%E8%AE%BA%E7%B3%BB%E7%BB%9F/)


## 搜索
- [Hexo Next 主题中添加本地搜索功能](https://vic.kim/2019/05/22/Hexo%20Next%20%E4%B8%BB%E9%A2%98%E4%B8%AD%E6%B7%BB%E5%8A%A0%E6%9C%AC%E5%9C%B0%E6%90%9C%E7%B4%A2%E5%8A%9F%E8%83%BD/)

## SEO
- [【搜索优化】Hexo-next百度和谷歌搜索优化](http://www.ehcoo.com/seo.html)


## 友链
> 参考：
> - [Hexo-NexT 新增友链](https://tding.top/archives/73ce4e7.html)
> - [Hexo-NexT 自定义友链页面](https://www.yileaf.com/archives/c34c6e60.html)

1. 新建links页面：`hexo new page links`，则会在`/source/`下创建`/source/links/index.md`文件
2. 在侧边栏添加“友链”标签页：
   1. 在站点`_config.yml`文件中添加：
    ```md
    menu:
        links: /links  || fa fa-link
    ```
   2. 修改标签页名称为中文：在文件`hexo>theme>next>languages>zh-Hans.yml`中添加：
   ```md
   menu:
     links: 友链
   ```
3. 在`/links/index.md`文件中添加内容：
```md
---
title: 友链
type: links
---

<style>.links-content{margin-top:1rem}.link-navigation::after{content:" ";display:block;clear:both}.card{width:130px;font-size:1rem;padding:0;border-radius:4px;transition-duration:.15s;margin-bottom:1rem;display:block;float:left;box-shadow:0 2px 6px 0 rgba(0,0,0,.12);background:#f5f5f5}.card{margin-left:16px}@media(max-width:567px){.card{margin-left:16px;width:calc((100% - 16px)/2)}.card:nth-child(2n+1){margin-left:0}.card:not(:nth-child(2n+1)){margin-left:16px}}@media(min-width:567px){.card{margin-left:16px;width:calc((100% - 32px)/3)}.card:nth-child(3n+1){margin-left:0}.card:not(:nth-child(3n+1)){margin-left:16px}}@media(min-width:768px){.card{margin-left:16px;width:calc((100% - 48px)/4)}.card:nth-child(4n+1){margin-left:0}.card:not(:nth-child(4n+1)){margin-left:16px}}@media(min-width:1200px){.card{margin-left:16px;width:calc((100% - 64px)/5)}.card:nth-child(5n+1){margin-left:0}.card:not(:nth-child(5n+1)){margin-left:16px}}.card:hover{transform:scale(1.1);box-shadow:0 2px 6px 0 rgba(0,0,0,.12),0 0 6px 0 rgba(0,0,0,.04)}.card .thumb{width:100%;height:0;padding-bottom:100%;background-size:100% 100%!important}.posts-expand .post-body img{margin:0;padding:0;border:0}.card .card-header{display:block;text-align:center;padding:1rem .25rem;font-weight:500;color:#333;white-space:normal}.card .card-header a{font-style:normal;color:#2bbc8a;font-weight:700;text-decoration:none;border:0}.card .card-header a:hover{color:#d480aa;text-decoration:none;border:0}</style><div><div class="links-content"><div class="link-navigation" id="links1"></div></div></div>


```


# Markdown

## Font-matter区
Font-matter区即 两行`---`之间的内容

## 文章置顶
- 在Font-matter区添加`toc: true`或`toc: 数字`
  - 数字越大越靠前，默认不设置则为0
  - 数值相同时按创建时间倒序排列

- [hexo个性化设置](https://mongolian.github.io/2018/07/16/Hexo%E7%BE%8E%E5%8C%96/)

## 文章摘要
若想要在博客首页只展示文章的摘要，在摘要与正文中间插入
```md
<!--more-->
```


## 引用站内文章
```md
{% post_link 站内文章对应的.md名称（不带后缀.md） %}
```
如：引用站内文章《{% post_link Restart %}》，对应的文件名是`Restart.md`
```md
{% post_link Restart %}
```

## 引用文章内锚点

如：跳转到本文的`##代码高亮`小节[代码高亮](#代码高亮)
```md
[代码高亮](#代码高亮)
```

- 如果描点名称中有空格或下划线`_`等字符串，应将其改为分隔符`-`


## 居中引用
- [Hexo-NexT Tag 插件的使用](https://tding.top/archives/29bfe8c9.html)

```md
{% cq %}居中引用{% endcq %}
```
效果：

{% cq %}居中引用{% endcq %}

## 字体
```md
<font face="字体" size="字号" color="颜色">这里是需要突出显示的内容</font>
```

## 插入图片
参考：[hexo引用本地图片无法显示](https://blog.csdn.net/xjm850552586/article/details/84101345)

Markdown语法：
```md
<meta name="referrer" content="no-referrer" />
{% asset_img inorder.png 中序遍历 %}
```
或
```md
![title](name.jpg)
```
或
```md
<img src="/asset/[your_image]" width="[width]" height="[height]" alt="[alternative_text]" title="[title]">
```

> - 在Mardown文件头部加入`<meta name="referrer" content="no-referrer" />`可解决图片不显示的问题
> - 头部是指`--`区域以后、正文之前

## 表格
- [Markdown表格中换行、合并单元格](https://www.jianshu.com/p/b6c85800c44e)
- 插入表格：
```md
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
      <th>标题行</th>
      <th>第一列标题</th>
      <th>第二列标题</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>第一行</th>
      <td>$a_{11}$</td>
      <td>$a_{12}$</td>
    </tr>
    <tr>
      <th rowspan="2">合并两行单元格</th>
      <td>$a_{21}$</td>
      <td>$a_{22}$</td>
    </tr>
    <tr>
      <td>第三行就少一个单元格</td>
      <td>$a_{32}$</td>
    </tr>
    <tr>
      <th>第四行</th>
      <td colspan="2">合并两列</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>
```

效果：
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
      <th>标题行</th>
      <th>第一列标题</th>
      <th>第二列标题</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>第一行</th>
      <td>$a_{11}$</td>
      <td>$a_{12}$</td>
    </tr>
    <tr>
      <th rowspan="2">合并两行单元格</th>
      <td>$a_{21}$</td>
      <td>$a_{22}$</td>
    </tr>
    <tr>
      <td>第三行就少一个单元格</td>
      <td>$a_{32}$</td>
    </tr>
    <tr>
      <th>第四行</th>
      <td colspan="2">合并两列</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

## 公式
- 公式对齐
```md
\begin{equation}
\begin{aligned}
第一行 &= 对齐符号为\& 然后两个斜杠换行\\
第二 &+ 左边有个对齐符 \\
三 &+ 第二三行的加号与第一行的等号对齐
\end{aligned}
\end{equation}
```
效果如下：
\begin{equation}
\begin{aligned}
第一行 &= 对齐符号为\& 然后两个斜杠换行\\
第二 &+ 左边有个对齐符 \\
三 &+ 第二三行的加号与第一行的等号对齐
\end{aligned}
\end{equation}

## 其他
### 提示块
- [Hexo-NexT Tag 插件的使用](https://tding.top/archives/29bfe8c9.html)

```md
{% note default %}
default 提示块标签
{% endnote %}

{% note primary %}
primary 提示块标签
{% endnote %}

{% note success %}
success 提示块标签
{% endnote %}

{% note info %}
info 提示块标签
{% endnote %}

{% note warning %}
warning 提示块标签
{% endnote %}

{% note danger %}
danger 提示块标签
{% endnote %}
```

在主题配置文件`_config.yml`中修改配置:
```yml
# Note tag (bs-callout)
note:
  # Note tag style values:
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: flat  ## 风格
  icons: false ## 要不要图标
  border_radius: 3  ## 圆角矩形
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0
```

效果：
{% note default %}
default 提示块标签
{% endnote %}

{% note primary %}
primary 提示块标签
{% endnote %}

{% note success %}
success 提示块标签
{% endnote %}

{% note info %}
info 提示块标签
{% endnote %}

{% note warning %}
warning 提示块标签
{% endnote %}

{% note danger %}
danger 提示块标签
{% endnote %}

```md
{% note warning %}
定义
{% endnote %}
```

{% note warning %}
定义
{% endnote %}

```md
{% note danger %}
算法
{% endnote %}
```

{% note danger %}
算法
{% endnote %}

```md
{% note success %}
定理/性质
{% endnote %}
```

{% note success %}
定理/性质
{% endnote %}

```md
{% note default %}
证明 或 推荐 或 引用 或 示例
{% endnote %}
```

{% note default %}
证明 或 推荐 或 引用 或 示例
{% endnote %}


# 其他
## github.io无法访问
- 2021.04.18更新：一回学校，就无法访问github.io，原先参照[解决无法访问github pages的方法](https://juejin.im/post/6863358382410203144)修改DNS可以解决“github.io无法访问”的问题，现该方法已失效。尝试了多种方法，最终[修改hosts文件](https://www.cnblogs.com/hudunyu/p/14346032.html)的方法奏效：
  - 通过[https://tools.ipip.net/dns.php](https://tools.ipip.net/dns.php)查询博客的ip，在第一个输入框输入`name.github.io`（比如本博客地址为`shootingwang.github.io`），回车即可在“解析IP”列得到4个IP地址
  - 将IP地址粘贴到hosts文件中，并附上博客地址`name.github.io`
```
ip地址1 name.github.io
ip地址2 name.github.io
ip地址3 name.github.io
ip地址4 name.github.io
```
  - 保存hosts文件（直接修改hosts文件可能会因权限问题无法保存，可以将hosts文件先复制粘贴到桌面，在桌面完成文件修改并保存后，再将hosts文件复制粘贴到原来的文件夹里）
  - <font color=gray>（MacOS无需此步骤）</font>刷新DNS：通过`Win`+`R`打开“运行”，输入`cmd`打开命令提示符，输入`ipconfig/flushdns`，回车即可
  - 即可正常打开github.io网页

> - Windows中，hosts文件的地址一般为：`C:\Windows\System32\drivers\etc`
> - MacOs中，hosts文件的地址一般为：`磁盘\private\etc`


# 参考资料
