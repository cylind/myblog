---
title: 利用tmux和htop保持GoogleCloudShell会话活跃
date: 2021-02-19 16:42:25
tags: 奇技淫巧
---

Goole Cloud Shell真是不错，配置高，性能好，唯一的缺点是会话容易断线。本文讲述一个保持GCS会话活跃的小技巧。

俺发现只要htop在运行，GCS回话就会保持活跃，就不会断线。所以，只要咱们保持htop运行，就可以放心地干其他的事了，那自然就想到多路会话和终端分屏。而tmux是一个比较好用的多路会话软件，下面演示如何操作。

<!-- more -->

htop和tmux貌似都是预装在GCS分配的虚拟机上的，如果没有就执行如下命令安装

```shell
sudo apt install htop tmux -y
```

然后利用tmux将终端分屏

```shell
tmux split-window -h
```

上面的参数 `-h` 表示垂直分屏，缺省则水平分屏。

接着，运行htop，很简单

```shell
htop
```

 最后，在窗口间移动光标，按下`Ctrl + b +o`即可将光标移到另一个窗口，然后就可以开心的干其他事了，而不用担心会话会突然断掉。