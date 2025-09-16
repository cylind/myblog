---
title: Markdown基本用法
tags: Hexo
math: true
abbrlink: 45104
date: 2020-02-09 15:33:06
---

Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档，其语法简洁，易学易用，功能强大，颇受青睐。Markdown写作软件推荐Typora，界面简洁，功能强大，挺不错。

<!-- more -->

## 标题

```
# 一级标题
## 二级标题
### 三级标题
```

注意 最后一个# 后要有空格隔开，列表也是一样。字体的加粗、倾斜、高亮、删除线、上下标则不要加空格。

## 字体

### 倾斜

` *text*`
*text*

###  加粗

`**text**`
**text**

###  倾斜加粗 

`***text***`
***text***

### 字体高亮

注意等号后面不要接空格。markdown语法下的高亮以及上下标功能在这个博客上没有显示出来，实际上本地Markdown软件是可以显示出来的。
```
markdown语法：
==字体高亮==

html标签：
<mark>字体高亮</mark>
```

==字体高亮==

<mark>字体高亮</mark>

### 上标下标

```
markdown语法：
上标用^ ^包围
2^2^
下标用~ ~包围
H~2~O

html标签：
2<sup>3</sup>
H<sub>2</sub>O
```

2^2^

H~2~O

2<sup>3</sup>
H<sub>2</sub>O

### 字体删除线

`~~text~~`
~~text~~

## 段落

### 引用

` 一级引用>,二级引用>>,...`  必须在行首用才行

```markdown
> text
> > sub_text
> > > more_text
```

> text
> > sub_text
> >
> > > more_text


### 分割线

` ---三个以上`且要在行首

---

上下各一条分割线

---

### 插入图片

` ![图片底部要显示的文本](link "图片标题")`  其中的link可以是超链接也可以是本地文件路径，绝对路径相、对路径都可以。

```
 ![Markdown基本语法](https://upload-images.jianshu.io/upload_images/643065-907bae9afdec50d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640/format/webp "markdown语法表")
```

  ![Markdown基本语法](https://upload-images.jianshu.io/upload_images/643065-907bae9afdec50d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640/format/webp "markdown语法表")

### 插入链接

` [text](link)`
```
[🔗简书markdown语法](https://www.jianshu.com/p/191d1e21f7ed)
```
[🔗简书markdown语法](https://www.jianshu.com/p/191d1e21f7ed)

### 插入代码

代码块下面复制粘贴的话还是当作代码块处理

### 插入表格 

  ```markdown
  表头|表头|表头
  ---|:--:|---:
  内容|内容|内容
  内容|内容|内容
  
  第二行分割表头和内容。
  - 有一个就行，为了对齐，多加了几个
  文字默认居左
  -两边加：表示文字居中
  -右边加：表示文字居右
  注：原生的语法两边都要用 |包起来。此处省略
  ```
| 表头 | 表头 | 表头 |
| ---- | :--: | ---: |
| 内容 | 内容 | 内容 |
| 内容 | 内容 | 内容 |

### 插入列表

无序列表用 - + * 任何一种都可以
有序列表用数字加点

### 数学公式

#### 行内嵌

```markdown
内嵌数学公式 $\sum_{i=1}^{10}f(i)\,\,\text{thanks}$
```

效果： $\sum_{i=1}^{10}f(i)\,\,\text{thanks}$

#### 块状

```
块状数学公式
$$
∑i=110f(i)
$$
```

$$
∑i=110f(i)
$$

更多数学公式的使用参见https://www.zybuluo.com/codeep/note/163962

### 待办事项

```
- [x] 支持以 PDF 格式导出文稿
- [x] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
- [x] 新增 Todo 列表功能
- [ ] 修复 LaTex 公式渲染问题
- [ ] 新增 LaTex 公式编号功能
```

- [x] 支持以 PDF 格式导出文稿
- [x] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
- [x] 新增 Todo 列表功能
- [ ] 修复 LaTex 公式渲染问题
- [ ] 新增 LaTex 公式编号功能

## 进阶

### HTML标签

不在 Markdown 涵盖范围之内的标签，都可以直接在文档里面用 HTML 撰写。目前支持的 HTML 元素有：`<mark> <kbd> <b> <i> <em> <sup> <sub> <br>`等 ，如：

```
使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑
```

使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑

### 各类图表

流程图


```flow
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end

st->op->cond
cond(yes)->e
cond(no)->op
```

序列图：

```seq
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```

甘特图：

```gantt
    title 项目开发流程
    section 项目确定
        需求分析       :a1, 2016-06-22, 3d
        可行性报告     :after a1, 5d
        概念验证       : 5d
    section 项目实施
        概要设计      :2016-07-05  , 5d
        详细设计      :2016-07-08, 10d
        编码          :2016-07-15, 10d
        测试          :2016-07-22, 5d
    section 发布验收
        发布: 2d
        验收: 3d
```