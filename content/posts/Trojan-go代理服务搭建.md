---
title: Trojan-go代理服务搭建
tags:
  - 对抗封锁
  - 对抗审查
abbrlink: 42498
date: 2020-06-14 22:58:24
---

Trojan-Go可以说是集成了Trojan-gfw和V2ray的优点吧，简单如trojan-gfw却又强大如V2ray。

<!-- more -->

## 获取Trojan-Go

先到[Trojan-Go release页面](https://github.com/p4gefau1t/trojan-go/releases) 下载最新的预编译包，然后解压：

```shell
mkdir trojan-go
cd trojan-go
wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.7.8/trojan-go-linux-amd64.zip
unzip trojan-go-linux-amd64.zip
```

如果解压那步出错，可能是没安装`unzip`,先安装，再操作

## 获取TLS证书

这一步的前提是，有一个域名并且解析到该VPS上。

这里我们用`certbot`来申请证书，首先安装`certbot`:

```shell
apt install certbot
```

然后，申请证书（注意，这一步要用到443端口，确认当前443端口没有被占用）：

```
certbot certonly --standalone -d yourDomain.com
```

生成的证书文件保存在`/etc/letsencrypt/live/`文件夹下。

Let’s Encrypt 提供的证书只有90天的有效期，我们必须在证书到期之前，重新获取这些证书，证书更新命令是(同样的，执行证书更新是要用到443端口，请确保443端口没有被占用)：

```shell
certbot renew --dry-run
```

为了方便，可以利用linux下的crontab做一个定时任务，每两月自动更新一次证书：

```shell
0 3 * */2 * certbot renew --pre-hook "service nginx stop" --post-hook "service nginx start"
```

`--pre-hook` 这个参数表示执行更新操作之前要做的事情，因为 `--standalone` 模式的证书申请或更新是用到443端口，所以需要先停止 `nginx` 服务，解除端口占用。 `--post-hook` 这个参数表示执行更新操作完成后要做的事情，这里就恢复 `nginx` 服务的启用。

## 配置本地Web服务

想省事的也可以不配置，直接反代别人做好的现成的网站，比如`1.1.1.1` 

```
apt install nginx
service start nginx
```

至于网站伪装，可以直接到网站搜索一些网站模板挂上去，当然，也可以不做这一步。

## 配置Trojan-Go

可以直接修改开发者给出的范例文件，把域名，密码改成自己想设置的：

```shell
nano ~/trojan-go/example/server.json
nano ~/trojan-go/example/client.json
```

如果想使用websocket的话，还要额外加上：

```
"websocket": {
    "enabled": true,
    "path": "/ws_path",
    "hostname": "yourDomain.com"
    }
```

最好的成品是这样的，server端：

 <mark>没配置有本地web服务的可填在remote_addr处填`1.1.1.1`或其他现成网站网址</mark>

```json

{
    "run_type": "server",
    "local_addr": "0.0.0.0",
    "local_port": 443,
    "remote_addr": "127.0.0.1",
    "remote_port": 80,
    "password": [
        "passwd"
    ],
    "ssl": {
        "cert": "/etc/letsencrypt/live/yourDomain.com/fullchain.pem",
        "key": "/etc/letsencrypt/live/yourDomain.com/privkey.pem",
        "sni": "yourDomain.com"
    },
    "websocket": {
    "enabled": true,
    "path": "/ws_path",
    "hostname": "yourDomain.com"
    },
    "router":{
        "enabled": true,
        "block": [
            "geoip:private"
        ]
    }
}

```

client端：

```
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1081,
    "remote_addr": "yourDomain.com",
    "remote_port": 443,
    "password": [
        "passwd"
    ],
    "ssl": {
        "sni": "yourDomain.com"
    },
    "mux" :{
        "enabled": true
    },
    "websocket": {
    "enabled": true,
    "path": "/ws_path",
    "hostname": "yourDomain.com"
    },
    "router":{
        "enabled": true,
        "bypass": [
            "geoip:cn",
            "geoip:private",
            "geosite:cn",
            "geosite:geolocation-cn"
        ],
        "block": [
            "geosite:category-ads"
        ],
        "proxy": [
            "geosite:geolocation-!cn"
        ],
        "default_policy": "proxy"
    }
}
```

## 运行Trojan-Go

### server端：

```
cd ~/trojan-go
nohup ./trojan-go -config ./example/server.json &
```

如果安装有joker 就更方便了，`joker ./trojan-go -config ./example/server.json`

### client端：

这里以Windows举例，先到[Trojan-Go release页面](https://github.com/p4gefau1t/trojan-go/releases) 下载Windows下的客户端，解压，然后把刚才的client.json配置文件下载下来，放到里面，接着新建一个`start.cmd`批处理脚本，写入如下内容：

```
trojan-go.exe -config client.json
```

保存退出，运行这个cmd脚本就开启代理了，浏览器配合代理插件，就可以使用愉快的上网了。

