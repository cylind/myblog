---
title: iframe嵌入视频自适应
tags: Hexo
abbrlink: 62577
date: 2019-08-09 21:33:46
---

解决用iframe标签嵌入视频时，视频与页面不协调的问题。
<!-- more -->

1. 首先在`site\themes\next\source\css\_custom.stl`创建一个CSS类

   ```html
   <div class="aspect-ratio">
       <iframe src="//player.bilibili.com/player.html?aid=24287094&cid=40734416&page=1" scrolling="yes" border="0" frameborder="no" framespacing="1" allowfullscreen="true"></iframe>
   </div>
   ```

2. 其次，用一个aspect-ratio类的块内容把iframe包起来,再写到md文件中，如下

   ```html
   <div class="aspect-ratio">
       <iframe src="//player.bilibili.com/player.html?aid=24287094&cid=40734416&page=1" scrolling="yes" border="0" frameborder="no" framespacing="1" allowfullscreen="true"></iframe>
   </div>
   ```

3.  最后，效果如下

<iframe src="http://pan-yz.chaoxing.com/preview/showpreview_439134309035544576.html"></iframe>