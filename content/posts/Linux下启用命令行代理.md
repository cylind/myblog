---
title: Linux下启用命令行代理
tags: [Linux, 对抗封锁]
date: 2020-05-03 11:59:50
---

首先是配置一个科学上网的环境，可以通过安装目前比较流行的V2ray或ShadowsocksR来实现。其次，需要让命令行中执行的程序的流量走代理通道，这个可以通过安装proxychains来实现。最后，开始享受你的愉快探索之旅吧！

<!-- more -->

# 科学上网

## V2ray

### 安装

v2ray是不区分客户端和服务端版本的，安装v2ray后，我们通过修改配置文件的inbounds和outbounds规则，让它既可以做服务端也可以做客户端。

安装方法多种多样，这里介绍通过官方提供的脚本一键式安装的方法：

```bash
wget https://install.direct/go.sh
sudo bash go.sh
```

如果你的操作系统是debian/ubuntu的话，这个脚本默认还会设置开机自启v2ray服务。

开启和关闭v2ray的简单指令如下：

```bash
service v2ray start
service v2ray stop
service v2ray restart
service v2ray reload
```

### 配置

v2ray的配置文件是 `/etc/v2ray/config.json` , 可以参照网上的教程来修改这个配置文件，当然也有那种自动生成配置文件的在线工具，如果你不想那么麻烦的话，把自己在windows下用v2rayN图形化客户端生成的配置文件拿过去直接用也是没有问题的。

## ShadowsocksR

### 安装



```bash
git clone https://github.com/shadowsocksr/shadowsocksr.git
```

或者[点击这里](https://github.com/shadowsocksrr/shadowsocksr/releases) 下载打包好的文件,然后解压。

最后，初始化配置：

```bash
bash initcfg.sh
```

### 配置并启动

找到`user-config.json` 并修改配置，主要修改以下内容

```json
"server": "ssr.xxx.com",
"server_port": 443,
"local_address": "127.0.0.1", 
"local_port": 1081, 
"password": "xxxx",
"method": "chacha20-ietf",
"protocol": "auth_aes128_md5",
"protocol_param": "145487:vlQz38",
"obfs": "tls1.2_ticket_auth",
"obfs_param": "update.microsoft.com",
```

然后，切换到shadowsocks目录，会发现一个local.py文件，在这个目录下执行:

```bash
python local.py -d start
```

验证一下，是否已经成功了：

```bash
curl --socks5-hostname 127.0.0.1:1081 www.google.com
```

返回网页数据则说明成功了。

# 命令行代理

## proxychains

ubuntu下直接安装即可:

```bash
sudo apt install proxychains
```

它的配置文件路径为 `/etc/proxychains.conf` ，需要修改的内容在文件内容的最后面，默认是设置tor的配置，把它注释掉，并添加我们的本地代理地址,如：

```
socks5 127.0.0.1:1080
```

使用起来很简单，只需要在需要执行的命令前加上`proxychains` 即可，如：

```bash
proxychains wget https://www.google.com
```

