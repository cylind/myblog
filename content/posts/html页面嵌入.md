---
title: html5页面嵌入
tags: [Hexo]
date: 2019-08-10 13:25:55
---

几个常见的h5嵌入标签的属性及用法介绍，包括iframe标签、embed标签以及script标签。

<!-- more -->

# iframe标签

`<iframe>` 标签规定一个内联框架。一个内联框架被用来在当前 HTML 文档中嵌入另一个文档。所有主流浏览器都支持 `<iframe>` 标签。

## 属性

一些关于iframe的属性，注意以下仅是Hexo支持的的部分属性。
关于iframe的src属性：

<iframe srcdoc="<p>Hello world!</p>" src="demo_iframe_srcdoc.htm"></iframe>
|          属性          | 描述                                     |                              值                              |
| :--------------------: | :--------------------------------------- | :----------------------------------------------------------: |
|          name          | 规定 iframe 的名称。                     |                             name                             |
|         height         | 规定iframe的高度                         |                            pixels                            |
|         width          | 规定iframe的宽度                         |                            pixels                            |
|          src           | 规定iframe显示的url                      |                             url                              |
|       scrolling        | 规定是否在 iframe 中显示滚动条。         |                     yes<br/>no<br/>auto                      |
|         srcdoc         | 规定页面中的 HTML 内容显示在 iframe 中。 |                          html-code                           |
| seamless<br>(无缝衔接) | 规定 iframe 看起来像是父文档中的一部分。 |                           seamless                           |
|        sandbox         | 对 iframe的内容定义一系列额外的限制。    | allow-forms<br/>allow-same-origin<br/>allow-scripts<br/>allow-top-navigation |
|      marginheight      | 规定 iframe的顶部和底部的边距。          |                            pixels                            |
|      marginwidth       | 规定 iframe的左右侧的边距。              |                            pixels                            |
|      frameborder       | 规定是否显示 iframe>周围的边框。         |                            1<br>0                            |

## 使用

设置参数嵌入‘菜鸟教程相关页面’，代码如下：

```html
<iframe
name="菜鸟教程"
height= 2055
width= 800
seamless
frameborder=0
marginwidth=0
marginheight=0
scrolling="no"
src="https://www.runoob.com/tags/tag-iframe.html">
</iframe>
```

嵌入效果如下：
<iframe
name="菜鸟教程"
height= 2055
width= 800
seamless
frameborder=0
marginwidth=0
marginheight=0
scrolling="no"
src="https://www.runoob.com/tags/tag-iframe.html">
</iframe>

# embed标签

embed标签定义了一个容器，用来嵌入外部应用或者互动程序（插件）。所有主流浏览器都支持 embed 标签。

## 属性

一些关于embed的属性，注意以下仅是Hexo支持的的部分属性

| 属性   | 描述                                                         | 值        |
| ------ | ------------------------------------------------------------ | --------- |
| height | 规定嵌入内容的高度。                                         | pixels    |
| width  | 规定嵌入内容的宽度。                                         | pixels    |
| src    | 规定嵌入内容的url                                            | url       |
| typy   | 规定嵌入内容的 MIME 类型。<br/>注：MIME = Multipurpose Internet Mail Extensions。 | MIME_type |

## 使用

设置参数嵌入‘菜鸟教程相关页面’，代码如下：

<embed width=800 height=1150 src="https://www.runoob.com/tags/tag-embed.html">

# Script标签

script标签用于定义客户端脚本，比如 JavaScript。script元素既可包含脚本语句，也可以通过 "src" 属性指向外部脚本文件。JavaScript 通常用于图像操作、表单验证以及动态内容更改。所有主流浏览器都支持 script标签。

## 属性

如果使用 "src" 属性，则 `<script> `元素必须是空的。

| 属性                                                         | 描述                                                   | 值          |
| ------------------------------------------------------------ | ------------------------------------------------------ | ----------- |
| [async](https://www.runoob.com/tags/att-script-async.html)   | 规定异步执行脚本（仅适用于外部脚本）。                 | async       |
| [charset](https://www.runoob.com/tags/att-script-charset.html) | 规定在脚本中使用的字符编码（仅适用于外部脚本）。       | *charset*   |
| [defer](https://www.runoob.com/tags/att-script-defer.html)   | 规定当页面已完成解析后，执行脚本（仅适用于外部脚本）。 | defer       |
| [src](https://www.runoob.com/tags/att-script-src.html)       | 规定外部脚本的 URL。                                   | *URL*       |
| [type](https://www.runoob.com/tags/att-script-type.html)     | 规定脚本的 MIME 类型。                                 | *MIME-type* |

## 使用

代码：

```html
<script
type="text/javascript"
src="http://www.xiami.com/widget/player-single?uid=32329501&sid=1776238762&mode=js"></script>
```

效果：

<script 
type="text/javascript" 
src="http://www.xiami.com/widget/player-single?uid=32329501&sid=1776238762&mode=js"></script>