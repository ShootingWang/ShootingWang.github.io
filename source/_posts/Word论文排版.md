---
title: Word论文排版
date: 2020-12-19 21:47:36
tags: [Word,Endnote]
categories: Everything
---

<center></center>
<!--more-->

# 目录
- **自定义目录，并填充省略号等引导符**：输入章节文字，全选，单击右键，选择【段落】——【制表位】
  - 【制表位】 填写页码右侧的制表位，可填写34.26字符，可根据效果调整数值
  - 【对齐方式】 选择“右”
  - 【引导符】 一般选择第3种或第5种
- **自定义目录，插入页码**：将鼠标定位在前一步骤产生的引导符后，点击【引用】——【交叉引用】
  - 【引用类型】 选择“标题”
  - 【引用内容】 选择“页码”
  - 点击【插入】——【关闭】即可


# 章节
- **新章从奇数页开始**：将鼠标定位在上一章的结尾，点击【布局】——【分隔符】——【奇数页】（插入分节符并在下一奇数页上开始新节）

# 正文

## 表格
- 通常情况下，表格上下方与正文均需隔一行空行
- **插入表标题**：选中表格，点击【引用】——【插入题注】
  - 【标签】选择需要的标题前缀，如“表格”、“公式”、“图表”等，如果不存在想要的标签，可以点击【新建标签】新建标签
  - 【位置】选择标题置于表格上方或下方，一般选择“所选项目上方”
  - 【编号】设置表格编号的格式
- **切分表格第一个单元格**：选择第一个单元格，点击【开始】——【边框】选择“斜下框线”，然后在该单元格 先“右对齐”输入列标题，然后回车，“左对齐”输入行标题

## 脚注
- **插入脚注**：将鼠标定位到要插入脚注的地方，点击【引用】——【插入脚注】，即可。
- **修改脚注格式**：右键点击脚注内容，选择【脚注】，修改脚注的【编号格式】、【起始编号】、【应用更改】（确定该更改应用的范围）等
- **脚注分隔符取消缩进**：在【视图】中选择“草稿视图”，然后在【引用】中选择“显示备注”，Word下方即出现“脚注编辑框”，单选框中下拉选择“脚注分隔符”，然后将光标置于脚注分隔符前方，删除脚注分隔符前面的空格。最后，在【视图】中选择“页眉视图”恢复页面视图。

## 公式
### 插入公式
【插入】——【公式】——编辑公式
- 快捷键：`Alt` + `+=`（Windows）


### 自动编号
将光标置于**公式内**的末尾，输入`#()`，然后将光标置于括号内，点击【插入】——【插入域】：
- 【类别】选择“编号”
- 【域名】选择“AutoNum”
- 【选项】选择目标数字格式
点击【确认】即可。

### 分章节“半”自动编号
> 章序号是“第三章”等中文，而公式中的“分章节编号”要求全数字编号（如`3.1`）

**方式一**：
1. 结合[上一小节“自动编号”](###自动编号)，将光标置于**公式内**的末尾，输入`#()`
2. 在括号内，点击【引用】——【插入题注】
3. 若是本章节（如第三章）的第一个公式，则需先【新建标签】——【任意标签（方便区分各个章节即可）】；否则直接跳到下一步
4. 勾选“从题注中排除标签”，选择本章节对应的【标签】，单击【确定】即可

**方式二**：
1. 将光标置于**公式内**的末尾，输入`#()`
2. 在括号内，`Ctrl`+`F9`（Windows）或`Cmd`+`F9`（MacOS）快捷插入域
3. 输入`Seq`（不区分大小写）+`任意标签`（每章节对应同一个标签，同一个标签的域连续编号）
4. 点击`F9`退出域编辑状态

> 选择域，点击`F9`刷新域

- 方法二更为便捷


### 等号对齐
1. 编辑公式，在需要换行的地方，`Shift`+`Enter`，则会出现一个下箭头
2. 在需要对齐的等号或其他运算符号左边，单击右键，选择【公式对齐方式】（MacOS）即可


## 符号
### 罗马数字
【插入】——【数字】——【数字】填写需要的数字——【编号类型】选择罗马数字



# 页眉
- **去掉页眉中的横线**：全选页眉的文字或换行符，点击【开始】——边框选择“无框线”
- **偶数页页眉显示论文标题**：
- **奇数页页眉显示当前章节标题**：双击奇数页页眉的位置，勾选【奇偶页不同】，点击【域】，【域名】中选中“StyleRef”，点击【选项】——【样式】——选择【标题1】——【添加到域】，单击【确认】即可

# 页码
- **插入页码**：点击【插入】——【页码】

# 导出为pdf
## Windows
单击【文件】——【导出】——【创建PDF\XPS文档】——【创建PDF\XPS】，命名，保存即可

## MacOS
单击【文件】——【另存为】——【文件格式】选择（导出格式）“PDF”，命名，保存即可

# 参考文献

> 推荐使用[EndNote](https://www.endnote.com/)进行参考文献的插入、管理

# EndNote
## Output Style
### 安装
在Endnote官网可以查找并下载需要的Output Style格式文件。
> 网址：[https://endnote.com/downloads/styles/](https://endnote.com/downloads/styles/)

1. 在【Keyword】中输入关键词，在【Citation Style】中选择引用类型，点击【Research】进行搜索
2. 下载相应的格式文件
3. 双击下载的文件（则在Endnote中打开该文件）
4. 点击【File】选择【Choose As】，重命名格式文件名称，并保存【Save】
5. 点击【File】选择【Close Style】关闭文件

> 例：查找、下载并安装GB/T 7714格式文件
> - 在【Keyword】中输入“GB”，并搜索
> - 得到两个搜索结果：
>   - Chinese Standard GBT7714 (numeric)
>   - Chinese Standard GBT7714 (Author-Year)
> - 根据自己的引用格式（numric或Author-Year）选择相应的格式文件下载
> - 双击下载的文件，在Endnote中打开该文件
> - 点击【File】选择【Choose As】，重命名该格式文件名称，并点【Save】保存
> - 点击【File】选择【Close Style】关闭文件

### 使用
点击【Edit】——【Output Styles】——【Open Style Manager】，打开Output Styles管理器。
- 找到目标格式文件，勾选，则这些格式文件会显示在【Output Styles】中
- 在【Edit】——【Output Styles】中选择想要使用的格式文件

### 编辑
有些既有的格式文件并不能完全满足我们的格式要求，这时就需要手动编辑/修改格式文件。
> 点击【Edit】——【Output Styles】——【Edit "你选择的Style名称"】，打开Style编辑器

- **Citations**：
  - Templates：修改引用格式
  - Ambiguous Citations：引用不同文献、但引用文本一模一样时，如何区分文献
    - Add the title for different works by the same author：同一作者的不同文献引用中添加文献标题（可选`Full Title`或`Short Title`）
    - Add a letter after the year：在年份后添加a, b, c等字母区分（如：`2012a`、`2012b`）

### GBT 7714-2015格式调整
从Endnote官网下载的GB/T 7714格式文件中的格式并不完全符合[GB/T 7714-2015](http://www.doc88.com/p-1136638747314.html)的要求。

numeric版本：
- **Page Numbers**：修改文献列表中页码的格式，选择`Show the full range of pages(e.g. 123-125)`，完全显示起始页码
- **Journal Names**：`Journal Name Format`选择`Use full journal name`
- **Citations**：修改引用格式
  - Author Lists：
    - 将Author Separators中的`before last`、`before last in format: Author(Year)`的`and`改为`和`
    - Abbreviated Author List - First Appearance：选择第二种—— If `3` or more authors, list the first `1` author(s) and abbreviated with: `等`（若文献作者数大于等于3个，则只显示第一作者并以“等”结尾）
    - Abbreviated Author List - Subsequent Appearances：选择第二种—— If `3` or more authors, list the first `1` author(s) and abbreviated with: `等`
  - 其他无需改动
- **Bibliography**：



## 下载引用文本
如何下载引用文本：
- [百度学术](https://xueshu.baidu.com)：搜索目标文献，点击【引用】，选择引用格式，点击【导入链接】中的“EndNote”，即可下载.enw格式的引用文本
- 知网：点击【引用】——【更多引用格式】——【文献导出格式】选择“EndNote”——【导出】，即可下载.txt格式的引用文本

## 导入引用文本
点击【File】——【Import】，选择前面下载的引用文本文件，单击【Options】，修改【Import Options】，然后点击“Import”即可。

|引用文本文件格式|`Import Options`选项设置|
|:---:|:----|
|.txt|Tab Delimited|
|.enw|EndNote Import|
|.ris|Reference Manager(RIS)|



## 同步Library
单击图标“循环转圈圈”【Sync Library】，同步本地内容到online

## Attach PDF
将PDF与Reference连接到一起，便于根据Reference列表查找、阅读文献。鼠标选中目标Reference条目，单击“回形针”图标【Attach PDF】，选择本地PDF文件，即可将PDF绑定到相应的Reference条目。
> - 记得保存 并 及时Sync 哦~
- 在EndNote中阅读文件，可以使用编辑工具进行高亮注释、添加文本注释等，非常方便呢

# 参考资料
- [如何设置硕士论文格式+奇偶页眉+页码+目录+三线表](https://wenku.baidu.com/view/6b94d6a3b14e852458fb57bb?pcf=2&bfetype=new&bfetype=new)