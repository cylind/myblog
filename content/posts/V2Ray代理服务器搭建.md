---
title: V2Ray代理服务器搭建
tags: [对抗封锁]
date: 2020-05-15 18:53:51
---

记录一次在vps上安装并配置v2ray的过程。

<!-- more -->

## 安装

v2ray是不区分客户端和服务端版本的，安装v2ray后，我们通过修改配置文件的inbounds和outbounds规则，让它既可以做服务端也可以做客户端。

安装方法多种多样，这里介绍通过官方提供的脚本一键式安装的方法：

```bash
wget https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh
chmod +x ./install-release.sh
./install-release.sh
```

如果你的操作系统是debian/ubuntu的话，这个脚本默认还会设置开机自启v2ray服务。

开启和关闭v2ray的简单指令如下：

```bash
service v2ray start
service v2ray stop
service v2ray restart
service v2ray reload
```

## 配置


### 申请证书

这一步的前提是，有一个域名并且解析到该VPS上。

这里我们用`certbot`来申请证书，首先安装`certbot`:

```bash
apt install certbot
```

然后，申请证书（注意，这一步要用到80和443端口，确认当前80和443端口没有被占用，且防火墙开放这两个端口）：

```sh
certbot certonly --standalone -d yourDomain.com
```

生成的证书文件保存在`/etc/letsencrypt/live/`文件夹下。

Let’s Encrypt 提供的证书只有90天的有效期，我们必须在证书到期之前，重新获取这些证书，证书更新命令是(同样的，执行证书更新是要用到443端口，请确保443端口没有被占用)：

```bash
certbot renew --dry-run
```

为了方便，可以利用linux下的crontab做一个定时任务，每两月自动更新一次证书：

```bash
0 3 * */2 * certbot renew --pre-hook "service nginx stop" --post-hook "service nginx start"
```

`--pre-hook` 这个参数表示执行更新操作之前要做的事情，因为 `--standalone` 模式的证书申请或更新是用到443端口，所以需要先停止 `nginx` 服务，解除端口占用。 `--post-hook` 这个参数表示执行更新操作完成后要做的事情，这里就恢复 `nginx` 服务的启用。

### V2ray配置

v2ray服务端的配置文件是 `/usr/local/etc/v2ray/config.json` 。这里只给出服务端的配置，客户端的配置建议使用V2rayN直接填写就好, 其中alertId默认为0，填好了可以导出vmess_url或者配置文件。

```json
{
"inbound": {
    "listen": "127.0.0.1",
    "port": 9000,
    "protocol": "vmess",
    "settings": {"id": "f8360bea-e83c-11eb-a90e-00fff1ff6342"},
    "streamSettings": {
        "network": "ws",
        "wsSettings": {"path": "/ws_path"}
    }
},
"outbound": {"protocol": "freedom"}
}
```


### Nginx配置

#### 服务器尚未部署有网站

在Nginx中配置文件以转发流量到V2ray上，在`etc/nginx/conf.d`下新建一个配置文件，比如`default.conf` ，写入如下内容(自行替换yourDomain.com)：

```
server {
    ### 1:
    server_name yourDomain.com;

    listen 80 reuseport fastopen=10;
    rewrite ^(.*) https://$server_name$1 permanent;
    if ($request_method  !~ ^(POST|GET)$) { return  501; }
    autoindex off;
    server_tokens off;
}

server {
    ### 2:
    ssl_certificate /etc/letsencrypt/live/yourDomain.com/fullchain.pem;

    ### 3:
    ssl_certificate_key /etc/letsencrypt/live/yourDomain.com/privkey.pem;

    ### 4:
    location /ws_path
    {
        proxy_pass http://127.0.0.1:9000;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_requests 10000;
        keepalive_timeout 2h;
        proxy_buffering off;
    }

    listen 443 ssl reuseport fastopen=10;
    server_name $server_name;
    charset utf-8;

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_requests 10000;
    keepalive_timeout 2h;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-256-GCM-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_ecdh_curve secp384r1;
    ssl_prefer_server_ciphers off;

    ssl_session_cache shared:SSL:60m;
    ssl_session_timeout 1d;
    ssl_session_tickets off;
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 10s;

    if ($request_method  !~ ^(POST|GET)$) { return 501; }
    add_header X-Frame-Options DENY;
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options nosniff;
    add_header Strict-Transport-Security max-age=31536000 always;
    autoindex off;
    server_tokens off;

    index index.html index.htm  index.php;
    location ~ .*\.(js|jpg|JPG|jpeg|JPEG|css|bmp|gif|GIF|png)$ { access_log off; }
    location / { index index.html; }
}
```

上面配置文件中，带有###号的是需要修改。第1,2,3处改为自己的域名，第4处改为自己想设置的path和端口，改完并保存配置文件后，检查nginx配置文件是否有误，并启动Nginx：

```sh
nginx -t
service nginx start
```

此外，为了进一步提高v2ray的隐蔽性，可以选择进行网站伪装，在网上找一个网站模板，解压到`/usr/share/nginx/html`目录下。

#### 服务器上已经部署有网站

若是服务器上已经搭建有了一个网站，只需要在该网站的Nginx配置文件里添加一个如下location即可:

```nginx
location /ws_path {
    if ($http_upgrade != "websocket") {
        return 404;
    }
    proxy_redirect off;
    proxy_pass http://127.0.0.1:9000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
  }
```


### 开启CDN（可选）

开启为域名开启CDN可以隐藏vps的真实ip，可以有效防止ip被墙或复活被墙ip，不过有点vps开启CDN后延迟会大大增加，但是对美西节点影响不大，如果是美西节点，比如洛杉矶，建议开启。

实现的方法很简单，把域名迁移到Cloudflare管理，并开启CDN加速代理。

## 实用工具

### 配置生成

[V2Ray配置在线生成](https://www.veekxt.com/utils/v2ray_gen)

[Nginx配置在线生成](https://www.digitalocean.com/community/tools/nginx)

 ### 订阅转换

https://acl4ssr.netlify.app/

https://bianyuan.xyz/

## 参考：

https://www.v2fly.org/

https://guide.v2fly.org/

https://printempw.github.io/v2ray-ws-tls-cloudflare/