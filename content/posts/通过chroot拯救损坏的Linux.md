---
title: 通过chroot拯救损坏的Linux
tags: Linux
abbrlink: 34289
date: 2020-11-01 15:41:50
---

初次接触Linux的时候，安装的是Manjaro（Deepin Desktop），一款基于Arch的桌面发行版，Arch采取的是滚动更新。后来滚动更新完后，时不时出现问题，有时开不了机。大部分都是依赖关系或者包冲突导致，我当时的做法是，先用一个U盘启动Manjaro LiveCD，然后执行chroot切换有问题的Manjaro系统分区作为`/`，最后升级/更新一次系统或利用timeshift进行系统还原。

<!-- more -->

### 进入LiveCD

将同样的发行版linux系统刻录到u盘，插入计算机，开机，按f12或其他，选择进入livecd

### 挂载Linux分区

切换到root

```
sudo -i
```

查看Linux分区位置

```
fdisk -l
```

挂载Linux分区到/mnt目录

```
# 假设linux分区为/dev/sda
mount /dev/sda /mnt
```

挂载其它必要环境或设备

```
for i in /dev /dev/pts /proc /sys /run;do mount -B $i /mnt$i;done
```

### chroot并修复

```
chroot /mnt
```

开始修复工作

### 参考

https://ziqiangxu.github.io/blog/accumulation/%E7%94%A8chroot%E4%BF%AE%E5%A4%8DLinux%E7%B3%BB%E7%BB%9F.html

https://www.cnblogs.com/tsreaper/p/chroot-fix-boot.html