
*   [前言](#_4)
*   [一、Markdown 是什么](#Markdown_11)
*   [二、Markdown 优点](#Markdown_26)
*   [三、Markdown 的基本语法](#Markdown_37)
*   *   [3.1 标题](#31__38)
    *   [3.2 字体](#32__55)
    *   [3.3 换行](#33__70)
    *   [3.4 引用](#34__75)
    *   [3.5 链接](#35__96)
    *   [3.6 图片](#36__111)
    *   [3.7 列表](#37__135)
    *   [3.8 分割线](#38__180)
    *   [3.9 删除线](#39__200)
    *   [3.10 下划线](#310__209)
    *   [3.11 代码块](#311__218)
    *   [3.12 表格](#312__259)
    *   [3.13 脚注](#313__281)
    *   [3.14 特殊符号](#314__296)
*   [四、Markdown 的高级用法](#Markdown_315)
*   *   [4.1 个人看法](#41__316)
    *   [4.2 制作待办事项](#42__318)
    *   [4.3 书写公式](#43__335)
    *   [4.4 绘制流程图](#44__345)
    *   [4.5 绘制序列图](#45__361)
    *   [4.6 绘制甘特图](#46__372)
    *   [4.7 Html](#47_Html_397)
*   [五、Markdown 工具](#Markdown_442)
*   [六、总结](#_448)

## 一、Markdown 是什么

`Markdown` 是一种轻量级标记语言，创始人为约翰 · 格鲁伯（John Gruber）。  
`Markdown` 允许人们使用易读易写的纯文本格式编写文档，然后转换成有效的 HTML 文档。  
`Markdown` 编写的文档可以导出 HTML 、Word、图像、PDF、Epub 等多种格式的文档。  
`Markdown` 编写的文档后缀为 .md, .markdown。  

推荐一款 [Markdown 编辑器](https://so.csdn.net/so/search?q=Markdown%20%E7%BC%96%E8%BE%91%E5%99%A8&spm=1001.2101.3001.7020) 
**Typora**下载链接：[Typora 下载](http://www.itmind.net/20343.html)。
## 二、Markdown 优点

*   纯文本编辑，只要是支持 Markdown 编辑的都能获得同样的结果，摆脱排版苦恼
*   学习成本低，常用的语法很少，简单易学快速上手
*   支持跨平台同步数据
*   支持插入图片、视频等
*   随时修改，不必担心 word 等工具出现排版错误
## 三、Markdown 的基本语法

### 3.1 标题

使用 #号标记，可以表示 1-6 级标题， 随 #的个数递增，一级标题字号最大，六级标题字号最小。  
代码如下：

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

效果如下：  

![](https://i-blog.csdnimg.cn/blog_migrate/21ac9e1f7ec824078c69480be427bb78.png)

注意：

*   最后一个`#`字符与标题中间留一个空格
*   标题应该置于行首，如果放入表格中可能无法正确解析

### 3.2 字体

星号与下划线都可以，单是斜体，双是粗体，三是粗斜体

<table><thead><tr><th align="center">代码</th><th align="center">效果</th></tr></thead><tbody><tr><td align="center"><code>*这是斜体*</code></td><td align="center"><em>这是斜体</em></td></tr><tr><td align="center"><code>_这是斜体_</code></td><td align="center"><em>这是斜体</em></td></tr><tr><td align="center"><code>**这是粗体**</code></td><td align="center"><strong>这是粗体</strong></td></tr><tr><td align="center"><code>__这是粗体__</code></td><td align="center"><strong>这是粗体</strong></td></tr><tr><td align="center"><code>***这是粗斜体***</code></td><td align="center"><em><strong>这是粗斜体</strong></em></td></tr><tr><td align="center"><code>___这是粗斜体___</code></td><td align="center"><em><strong>这是粗斜体</strong></em></td></tr></tbody></table>

快捷键：  
加粗 Ctrl+B  
斜体 Ctrl+I

### 3.3 换行

Markdown 换行的方式有很多种:

*   直接在一句话后敲两个空格
*   两句话之间加一个空行
*   如果你在编辑的时候，想让一行文字在显示的时候换行，就在中间加`<br/>`

### 3.4 引用

Markdown 中引用通过符号 `>` 来实现。`>` 符号后的空格，可有可无。  
在引用的区块内，允许换行存在，换行并不会终止引用的区块。如果要结束引用，需要一行空白行，来结束引用的区块。  
代码：

```
>这是一个引用
```

效果

这是一个引用  
此外，引用还可以嵌套使用：  
代码：

```
>这是一个引用：
>>这是一个引用的引用
>>>这是一个引用的引用的引用
```

效果：

这是一个引用：

这是一个引用的引用

这是一个引用的引用的引用

### 3.5 链接

Markdown 中插入链接的使用方式是：  
代码：

```
[链接名称](链接地址)
<链接地址>
即是：
[这是小白的主页](https://blog.csdn.net/qq_40818172?type=lately)
或者
<https://blog.csdn.net/qq_40818172?type=lately>
```

效果：

[这是小白的主页](https://blog.csdn.net/qq_40818172?type=lately)  
[https://blog.csdn.net/qq_40818172?type=lately](https://blog.csdn.net/qq_40818172?type=lately)

### 3.6 图片

Markdown 中插入图片的使用方式是：  
代码：

```
![图片描述，可写可不写，但是中括号要有](图片地址，本地链接或者URL地址。)
比如我此文章的图片：
![卷不动的小白](https://i-blog.csdnimg.cn/blog_migrate/ec1c9a9c57e4d6494b5b36554f7af692.png)
)
```

效果：  

![](https://i-blog.csdnimg.cn/blog_migrate/ec1c9a9c57e4d6494b5b36554f7af692.png)

  
也可以`修改位置和图片大小`：  
代码：

```
![图片描述，可写可不写，但是中括号要有](图片地址，本地链接或者URL地址#pic_center空格=长x宽)
比如我此文章的图片：
![卷不动的小白](https://i-blog.csdnimg.cn/blog_migrate/ec1c9a9c57e4d6494b5b36554f7af692.png#pic_center =60x60)
)
```

效果：  

![](https://i-blog.csdnimg.cn/blog_migrate/ec1c9a9c57e4d6494b5b36554f7af692.png#pic_center)

  
`注意：等号前有空格，是x不是*`

博主自己经常 Ctrl+v 粘贴图片更为便捷

### 3.7 列表

列表分为有序列表和无序列表

*   无序列表，使用`*`、`+`、`-`，再加一个空格作为列表的标记
*   有序列表，使用数字并加上`.`号，再加一个空格作为列表的标记  
    代码：

```
* 无序列表 1
+ 无序列表 2
- 无序列表 3
1. 有序列表 1
2. 有序列表 2
3. 有序列表 3
```

效果：

*   无序列表 1

*   无序列表 2

*   无序列表 3

1.  有序列表 1
2.  有序列表 2
3.  有序列表 3

如果想要控制列表的层级，则需要在列表符号前使用`Tab`  
代码：

```
+ 无序列表 1
+ 无序列表 2
	+ 无序列表 2.1
	+ 无序列表 2.2

1. 有序列表 1
	1.1 有序列表 1.1
2. 有序列表 2
	2.1 有序列表2.1
```

效果：

*   无序列表 1
*   无序列表 2
    *   无序列表 2.1
    *   无序列表 2.2

1.  有序列表 1
    1.  有序列表 1.1
2.  有序列表 2
    1.  有序列表 2.1

### 3.8 分割线

Markdown 中给出了多种分割线的样式，我们可以使用分割线让文章结构更加的清晰。  
分割线的使用，可以在一行中用三个`-`or`*`来建立一个分割线，但是注意：在分割线的上面空一行！！！

代码：

```
分割线：

---
***
- - -
* * *
```

效果：

* * *

* * *

* * *

* * *

注意：写分割线前，要空一行之后写，否则会导致前一行字体放大。

### 3.9 删除线

删除线的的使用，可以在要添加删除线的文字前后添加两个`~`  
代码：

```
~~这是要被删除的文字~~
```

效果：

~~这是要被删除的文字~~

### 3.10 下划线

下划线的使用和 html 中类似，在需要添加下划线的文字首尾添加`<u>文本</u>`  
代码：

```
<u>这行文字已被添加下划线</u>
```

效果：

<u>这行文字已被添加下划线</u>

### 3.11 代码块

Markdown 中代码块有两种：  
如果在一行内需要引用代码，只需要用反引号 ` 引起来就好了。  
代码：

```
`Hello` World.
```

效果：

`Hello` World.

如果是在一个块内需要引用代码，则在需要引用的代码块的前一行和后一行使用三个反引号，同时在前一个反引号后写入代码的语言。  
代码：  

![](https://i-blog.csdnimg.cn/blog_migrate/b35dfa9609fc00d8a35e44d98e3b7a81.png)

  
效果：

```
#include<iostream>
int main(){
   printf("HelloWorld");
}
```

支持以下语言：

```
bash
c，clojure，cpp，cs，css
dart，dockerfile, diff
erlang
go，gradle，groovy
haskell
java，javascript，json，julia
kotlin
lisp，lua
makefile，markdown，matlab
objectivec
perl，php，python
r，ruby，rust
scala，shell，sql，swift
tex，typescript
verilog，vhdl
xml
yaml
```

### 3.12 表格

表格使用`|`来分割不同的单元格，使用`-`来分隔表头和其他行

*   `:-`：将表头及单元格内容左对齐
*   `-:`：将表头及单元格内容右对齐
*   `:-:`：将表头及单元格内容居中

代码：

```
| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |
```

效果：

| 项目  |     价格 | 数量  |
| --- | -----: | :-: |
| 计算机 | \$1600 |  5  |
| 手机  |   \$12 | 12  |
| 管线  |    \$1 | 234 |

### 3.13 脚注

脚注是对文本的备注，我们时长在论文中看到脚注，在 Markdown 中的使用方法  
代码：

```
使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2], 你可以使用 Typora[^T] 编辑器进行书写。
[^1]:Markdown是一种纯文本标记语言
[^2]:HyperText Markup Language 超文本标记语言
[^T]:NEW WAY TO READ & WRITE MARKDOWN.
```

效果：  
使用 Markdown[1](#fn1) 可以效率的书写文档, 直接转换成 HTML[2](#fn2),

注意：脚注自动被搬运到最后面，请到文章末尾查看，并且脚注后方的链接可以直接跳转回到加注的地方。

### 3.14 特殊符号

对于 Markdown 中的语法符号，前面家反斜线`\`即可以显示符号本身。  
代码：

```
\\
\*
\_
\+
\.
等等
```

效果：

\  
\*  
_  
\+  
.

## 四、Markdown 的高级用法

### 4.1 个人看法

`Markdown` 是非常厉害的，但是我认为它建立的初衷是为了方便大家记笔记写博客，它具有很强大的功能，例如流程图、复杂的公式呈现，虽然看起来很有用，但是我认为这些功能与它创立的初衷是违背的，而且做流程图和复杂的公式是有专门的工具，而且十分便捷。所以个人认为，`Markdown`的一些高级用法了解一下即可，博主也不是很会使用参考了其他资料稍微来整理一下笔记。此处只简要提一下，如果想要了解更多详细的高级用法：[菜鸟教程 Markdown 高级用法](https://www.runoob.com/markdown/md-advance.html)、[Cmd Markdown 简明语法手册](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#cmd-markdown-%E9%AB%98%E9%98%B6%E8%AF%AD%E6%B3%95%E6%89%8B%E5%86%8C)

### 4.2 制作待办事项

我们可以使用`Markdown`来制作一个待办事项，格式为、`-[]` 表示未完成；`-[x]`表示已完成  
代码：

```
- [ ] 支持以 PDF 格式导出文稿
- [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
- [x] 新增 Todo 列表功能
- [x] 修复 LaTex 公式渲染问题
- [x] 新增 LaTex 公式编号功能
```

效果：

*   [ ]  支持以 PDF 格式导出文稿
*   [ ]  改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
*   [x]  新增 Todo 列表功能
*   [x]  修复 LaTex 公式渲染问题
*   [x]  新增 LaTex 公式编号功能

### 4.3 书写公式

Markdown 支持书写公式，例如书写一个质能守恒公式。  
`$$`表示整行公式  
代码：

```
$$E=mc^2$$
```

效果：

E = m c 2 E=mc^2 E=mc2

### 4.4 绘制流程图

代码:  

![](https://i-blog.csdnimg.cn/blog_migrate/9694f02d0c0e830022b8d4121a2431b5.png)

  
效果：



```mermaid
flowchat
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end


st->op->cond
cond(yes)->e
cond(no)->op
```


### 4.5 绘制序列图

代码：  

![](https://i-blog.csdnimg.cn/blog_migrate/2cdfcfdc5c5222178391b77399632e9d.png)

效果：



### 4.6 绘制甘特图

代码：  

![](https://i-blog.csdnimg.cn/blog_migrate/f715523bdb6e01161a5a98f1001c9256.png)

效果：


如果感兴趣可以去 [Cmd Markdown 简明语法手册](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#cmd-markdown-%E9%AB%98%E9%98%B6%E8%AF%AD%E6%B3%95%E6%89%8B%E5%86%8C)这里学习更多。

### 4.7 Html

Markdown 支持原生`HTML`语法，譬如，你可以用 Html 写一个纵跨两行的表格：  
代码：

```
<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>
```

效果：

<table><tbody><tr><th rowspan="2">值班人员</th><th>星期一</th><th>星期二</th><th>星期三</th></tr><tr><td>李强</td><td>张明</td><td>王平</td></tr></tbody></table>

也可以实现对字体格式的改变

代码：

```
<font face="楷体" color=#00ffff size=5>改变文字格式</font>
```

效果：

改变文字格式

## 五、Markdown 工具

*   本地 APP：首推 Typora，当然还有其他一些好用的软件，我用的是 Typora；
*   国内博客平台：CSDN、简书、掘金、博客园、知乎等。  
    Typora 下载链接：[Typora 下载](http://www.itmind.net/20343.html)

## 六、总结

为什么要写这篇博客，不仅是为了分享我的学习过程，也是为了给自己记个笔记，哪里忘记了，回来再看一眼，也可以很快的回想起来。所以快快把 Markdown 语法学起来吧，一起加油！！！

1.  Markdown 是一种纯文本标记语言 [↩︎](#fnref1)
    
2.  HyperText Markup Language 超文本标记语言 [↩︎](#fnref2)