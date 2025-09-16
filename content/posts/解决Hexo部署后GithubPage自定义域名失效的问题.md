---
title: 解决Hexo部署后GithubPage自定义域名失效的问题
abbrlink: 63221
date: 2020-05-16 22:00:23
tags: Hexo
---

每次Hexo部署时，会把本地文件覆盖到Github上，如果是自己直接在Github上给GithubPage绑定域名的话，绑定域名生成的域名文件CNAME会被删除掉，所以会出现每次部署完Hexo后，域名就失效的情况。

<!-- more -->

解决方法也很简单，直接在本地博客的source文件夹创建CNAME域名文件，在里面写入自己要绑定的域名就可以啦，这样一来，每次用Hexo生成博客内容时，CNAME都会被放到所生成的博客内容的根目录下，部署的时候，自然就连同它一块传上去啦，这样再也不用担心部署完成后CNAME文件消失，域名捆绑失效的问题啦！

![](http://p.ananas.chaoxing.com/star3/origin/da97e70dc914c62f23c9d6cc71c6bbb9.png)

![](http://p.ananas.chaoxing.com/star3/origin/7c1062730e22098162d4362f343628f7.png)