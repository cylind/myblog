---
title: Google搜索技巧
date: 2021-02-05 10:54:20
tags: 奇技淫巧
---

**Google搜索**是由Google公司推出的一个互联网搜索引擎，它是互联网上最大、影响最广泛的搜索引擎。Google每日透过不同的服务，处理来自世界各地超过30亿次的查询。能熟练掌握一些相关的搜索技巧，会让你事半功倍。

<!-- more -->

经常使用VPN的朋友，可能会被Google重定向至不同的地区，要享受原汁原味的Google Search可以点击  https://www.google.com/ncr 以禁止国别转跳。

## 使用“与或非”逻辑词

1. 与，使用空格隔开多个关键词。比如`linux debian`，返回包含这两个关键词的搜索结果。
2. 或，使用`OR`连接多个关键词。比如`linux OR redhat`，返回仅包含linux或者仅包含redhat或者二者都包含的搜索结果，即返回的结果为兼或。
3. 非，使用`-`连接指定关键词。比如`linux -redhat，返回包含linux关键词但排除redhat关键词的搜索结果。

这些以上这三种用法还可以组合使用，已达到更精确的限制范围。

## 精确搜索和近似搜索

1. 使用`""`英文双引号可实现精确搜索，即搜索结果必需完整包含英文双引号内的关键词。比如`"install youtube-dl"`,返回结果都是完整包含这词组，包括空格。值得注意的是，搜索结果是忽略大小写的。
2. 使用`~`符号修饰关键词可实现近义词搜索，即搜索结果包含关键词本身及其近义词。比如`linux mint ~great`,返回结果包含`linux mint great`和`linux mint good`等。
3. 可使用`*`通配符来代替关键词中不确定的部分。比如`promgram thin*`，会有`promgram think`的搜索结果返回。
4. 使用`..`连接两个数字，表示位于两个数字之间的范围。比如`British prime minister 1920..1950`，返回结果中包含`1940`，`1927`等关键词。

## 使用操作算符

所有的这些输入的操作算符都是以相同的方式工作的，将这些算符作为你搜索请求的一部分输入，再将变量紧接在这些输入的操作算符之后的冒号之后（而不是空格），就像这样：`操作算符:变量`

| **算符**      | **用途**                                                     | 用法                   |
| ------------- | ------------------------------------------------------------ | ---------------------- |
| **filetype:** | 限制所搜索的文件一个特定的格式                               | **filetype:extension** |
| **inanchor:** | 限制搜索的词语是网页中链接内包含的关键词                     | **inanchor:keyword**   |
| **intext:**   | 限制搜索的词语是网页内文包含的关键词                         | **intext:keyword**     |
| **intitle:**  | 限制搜索的词语是网页标题中包含的关键词                       | **intitle:keyword**    |
| **inurl:**    | 限制搜索的网页的地址                                         | **inurl:keyword**      |
| **site:**     | 限制所进行的搜索在指定的域名或网站内                         | **site:domain**        |
| **related:**  | 限制所进行的搜索在与指定的网站相似的网站                     | **related:domain/url** |
| **link:**     | 返回所有链接到某个URL地址的网页                              | **link:url**           |
| **cache:**    | 返回GOOGLE服务器上某页面的快照                               | **cache:url**          |
| **info:**     | 显示与某链接相关的一系列搜寻，提供cache 、link 、related和完全包含该链接的网页的功能。 | **info:url**           |

## 其他用法

1. 用作词典。`define:某单词`
2. 用作计算器。`((1+2)*3)^2`

## 参考

https://www.williamlong.info/archives/728.html

https://www.imooc.com/article/4071

http://ecaaser3.ecaa.ntu.edu.tw/weifang/cea/%E5%96%84%E7%94%A8GOOGLE.htm

https://www.pcmag.com/how-to/23-google-search-tips-youll-want-to-learn