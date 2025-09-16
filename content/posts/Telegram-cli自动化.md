---
title: Telegram-cli自动化
tags: [Linux, 奇技淫巧]
date: 2020-05-08 13:06:00
---

<!-- more -->

## 安装Telegram-cli

先把仓库源码clone下来（更换为kenorb-contrib，源仓库无法build）:

```sh
git clone --recursive https://github.com/kenorb-contrib/tg.git && cd tg
```

再安装相关依赖 (ubuntu/debian)：

```sh
sudo apt-get install libreadline-dev libconfig-dev libssl-dev lua5.2 liblua5.2-dev libevent-dev libjansson-dev libpython-dev libpython3-dev libgcrypt-dev zlib1g-dev lua-lgi make -y
```

然后，开始编译:

```sh
./configure
make
```

<mark>换源后不会出现以下依赖问题，可忽略</mark>

这一步可能会出问题，如果在执行`./configure` 时报错`zlib error`, 则应通过安装zlib依赖来解决:

```bash
sudo apt install zlib1g-dev
```

如果是报`openssl error`, 则是因为openssl的api升级了，可通过安装如下指定版本号的libssl来解决：

```sh
sudo apt install libssl1.0-dev
```

## 使用

可通过执行如下代码启动Telegram-cli客户端，注意，要在科学上网环境下执行！

```bash
bin/telegram-cli -k tg-server.pub
```

如果没有配置好科学上网环境，启动之后是用不了的，可以通过`proxychains` 让命令行走代理(proxychains的配置之前有讲过，可以翻一下之前的文章)：

```sh
proxychains bin/telegram-cli -k tg-server.pub
```

第一次启动，会让你输入手机号，别忘了+86国际区号哦，然后一路验证。验证完成后就可以使用了，这里列出一些常用语法:

```sh
msg <peer> sometext #例如:msg @somebody hello
send_file <peer> file_path #例如:send_file @somebody /root/tg.log
help # 更多用法直接输入help
```

## 自动化

利用Linux下的crontab设置如下定时任务，可完成定时发消息的任务

```bash
telegram-cli_PATH/bin/telegram-cli -W -e "msg <peer> sometext"
```

