---
title: Debian-Ubuntu定时重启
tags: Linux
abbrlink: 39331
date: 2021-01-30 18:38:42
---

注意，要在root用户下，设置crontab，并且指定执行指令的绝对路径`/sbin/reboot`，而不是简单的写`reboot`，否则无效，因为只写reboot不会以root身份执行，权限不够的。

参考：

https://www.moerats.com/archives/623/

https://sb.sb/blog/debian-9-rc-local/

https://blog.csdn.net/kk3909/article/details/105025144