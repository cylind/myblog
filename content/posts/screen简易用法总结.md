---
title: screen简易用法总结
tags: Linux
abbrlink: 6948
date: 2020-09-27 17:28:13
---

screen是一个非常实用的工具，提供从单个 SSH 会话（或本地会话）中使用多个 shell 窗口（会话）的能力。当会话被分离或网络中断时，screen 各会话中启动的进程仍将运行，而且你可以随时重新连接到 某个指定screen 会话查看情况。

<!-- more -->

## 启动一个screen会话

可以简单的在命令行里直接输入`screen`，这样便启动并进入到一个screen会话了。

如果你想给这个screen会话做个标记，也就是给它一个名称，可以用`screen -S ssesion_name`替换上述的`screen`,这样就启动并进入到一个名为session_name的screen会话了。

## 在screen会话执行命令

进入screen会话后，便可在这个screen会话中执行一些你想执行的命令了，比如升级系统`apt update && apt upgrade -y`。

## 分离screen会话

如果你的命令执行时间比较长，比如上述的升级系统操作，而你又不想等待，想干点别的事。

这个时候你可以分离这个会话，让它在后台运行，按`Ctrl + a + d`即可分离当前会话，回到本地会话。

## 重返screen会话

当你觉得时间差不多了，想回到之前升级系统那个会话看看运行结果或运行情况，你可以输入`screen -r`返回之前的会话，前提是你只创建了一个screen会话，如果有多个，你还得知道会话的pid或者name。

可以输入`screen -ls`查看当前所有的screen会话，例如：

```shell
screen -ls

There are screens on:
7880.session    (Detached)
7934.session2   (Detached)
7907.session1   (Detached)
3 Sockets in /var/run/screen/S-root.
```

其中7880,7934,7907为各screen会话对应的pid，而session，session1，session2则为它们对应的name。

可以指定pid `screen -r 7934`或指定name `screen -r -S session2`回到指定的screen会话中。

## 结束screen会话

这个很简单，在screen会话中输入exit或者按`Ctrl + d`即可结束当前会话。



参考：

https://handerfly.github.io/linux/2019/03/31/Screan%E5%91%BD%E4%BB%A4%E7%9A%84%E4%BD%BF%E7%94%A8/