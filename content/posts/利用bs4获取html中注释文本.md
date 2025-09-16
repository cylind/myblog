---
title: 利用bs4获取html中注释文本
date: 2020-09-17 10:22:08
---

方法一：用string代替text

string:输出单一子标签文本内容或注释内容（选其一，标签中包含两种内容则输出为None）

strings: 返回所有子孙标签的文本内容的生成器（不包含注释）

stripped_strings:返回所有子孙标签的文本内容的生成器（不包含注释,并且在去掉了strings中的空行和空格）

text:只输出文本内容，可同时输出多个子标签内容

get_text():只输出文本内容，可同时输出多个子标签内容



方法二：调用bs库中的Comment类

findAll(text=lambda text: isinstance(text, Comment))

参考：

https://www.cnblogs.com/kongzhagen/p/8315204.html

https://blog.csdn.net/weixin_41710606/article/details/86089605