---
title: 修复破损locales
date: 2021-02-08 17:44:28
tags: Linux
---

使用cleanbit清理Linux Mint系统的日志、缓存、多余的本地化语言包，忘记去设置保留中文简体，结果只剩下英文了。

网上查了好久，开始想通过TimeShift备份的文件进行恢复，谁知道它locales文件夹里没有zh_hans这些语言包，和本地系统一样，只有个C.UTF-8和en。

备份系统后，试着复制文件去覆盖，结果没成功。。。

<!-- more -->

苦心人天不负，一番苦苦搜寻，我终于找到解决方法了。

```bash
# 先完全卸载
sudo apt-get purge locales
sudo apt-get purge language-pack-zh-hans
sudo apt-get purge language-pack-gnome-zh-hans
# 再重新安装
sudo apt-get install locales
sudo apt-get install language-pack-zh-hans
sudo apt-get install language-pack-gnome-zh-hans
```

还遗留的小问题是，部分应用和系统菜单没有恢复到中文，我估计是缓存没生效，或者这部分得另外配置。

## 参考

https://unix.stackexchange.com/questions/299536/locale-error-cannot-open-locale-definition-file-fa-ir