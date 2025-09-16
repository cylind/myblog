---
title: 利用nginx搭建文件服务器
tags:
  - vps
  - 奇技淫巧
abbrlink: 42784
date: 2020-06-13 14:01:23
---

有时候本地网络不是那么方便，需要先在服务器下载文件，然后再从服务器把文件取回来，那么这时候考虑在服务器上搭建个简单的文件服务器来传输文件还是个不错的选择呢。

<!-- more -->

## 添加server

修改/etc/nginx/conf.d下的conf文件，没有的话，新建一个，在里面加入一个server，内容如下：

```
server {
    ### 文件服务器
    listen       8090;
    listen       [::]:8090;
    server_name  localhost;
    
    autoindex on;
    autoindex_exact_size off;
    autoindex_localtime on;
    charset utf-8;
    location / {
        root   /root/share; ## 这里是你要分享的文件夹
        index  index.html index.htm;
    }
}
```



## 修改user

由于上面的索引文件夹是root用户所有，直接访问会403 forbidden的，所以要修改一下用户，打开/etc/nginx/nginx.conf,修改里面的开头部分的user项，把 `user www-data;` 改为 `user root;`

## 重启nginx

完成配置后，重启nginx使配置生效，`service nginx restart` ,如果开了防火墙，记得把刚才设置的端口通过一下。
