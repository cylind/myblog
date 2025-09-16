---
title: Tor使用教程
tags: 
  - 对抗审查
  - 对抗封锁
abbrlink: 60639
date: 2020-05-27 20:16:46
---

Tor是实现匿名通信的自由软件。其名源于“The Onion Router”（洋葱路由器）的英语缩写。用户可透过Tor接达由全球志愿者免费提供，包含7000+个中继的覆盖网络，从而达至隐藏用户真实地址、避免网络监控及流量分析的目的。Tor用户的互联网活动（包括浏览在线网站、帖子以及即时消息等通信形式）相对较难追踪。Tor的设计原意在于保障用户的个人隐私，以及不受监控地进行秘密通信的自由和能力。

<!-- more -->

使用Tor一般有两种方式，最常见的一种，是直接使用集成了Firefox浏览器的Tor Broswer，这种方法简单高效，不需要额外配置，直接下载安装即可使用。另一种，则稍显复杂，直接使用裸Tor。使用裸Tor可以更方便地搭配其他软件使用，而且占用资源少，更适合追求简洁的玩家。

下面就是介绍Windows/Linux下如何配置这种裸Tor来匿名上网。

## Windows下使用Tor

### 从Tor Broswer中提取Tor

去到Tor官网下载Tor浏览器安装到任意目录，安装完成后把安装目录下的`Tor Browser\Browser\TorBrowser\Tor`文件夹复制一份出来，在这个`Tor`文件夹下新建一个`Data`文件夹,再把`\Tor Browser\Browser\TorBrowser\Data\Tor`下的`geoip`、`geoip6`这两个文件复制到`Data`文件夹中，这样就得到了裸Tor包含的所有文件了。

接下来就是编写Tor的配置文件了，在`Tor`文件下创建`torrc`文件，用编辑器打开，输入配置信息，这里给出把tor作为前置代理的配置和利用meek插件(新版的Tor中，所有插件都包含在obfs4proxy.exe中)直连tor的配置：

```
# 把tor作为其他代理软件的前置代理
DataDirectory ./Data
GeoIPFile ./Data/geoip
GeoIPv6File ./Data/geoip6
# 设置前置socks5代理
Socks5Proxy 127.0.0.1:1080
# 排除有安全隐患的国家
ExcludeNodes {cn},{hk},{mo},{kp},{ir},{sy},{pk},{cu},{vn},{ru}
ExcludeExitNodes {cn},{hk},{mo},{sg},{th},{pk},{by},{ru},{ir},{vn},{ph},{my},{cu}
StrictNodes 1
```

```ini
# 利用meek插件直连tor的配置
DataDirectory ./Data
GeoIPFile ./Data/geoip
GeoIPv6File ./Data/geoip6
# 使用meek网桥
ClientTransportPlugin meek_lite exec ./PluggableTransports/obfs4proxy.exe
UseBridges 1
Bridge meek_lite 0.0.2.0:2 97700DFE9F483596DDA6264C4D7DF7641E1E39CE url=https://meek.azureedge.net/ front=ajax.aspnetcdn.com
# 排除出口节点
ExcludeExitNodes {cn},{hk},{mo},{sg},{th},{pk},{by},{ru},{ir},{vn},{ph},{my},{cu}
StrictNodes 1
```

### 直接下载Windows Expert Bundle

除了下载Tor浏览器提取裸Tor的方法外，还可以在这里https://www.torproject.org/download/tor/直接下载`Windows Expert Bundle`，解压后即可得裸Tor，不过这里的裸Tor是真的裸，连meek插件（obfs4proxy.exe）都是没有的，那么它自然是不能直连的，只能作为其它代理软件的前置代理了，配置文件写法如上，不过注意`geoip`、`geoip6`文件路径，不能照搬上述配置信息了，自己在配置文件中改一下路径。

### 在Windows下运行Tor

通过上述两个步骤获得Tor后，即可开始运行Tor。运行Tor可以在Tor文件下调出命令行输入`tor -f torrc`,为了方便直接在`Tor`文件夹下创建一个名为`start.cmd`的脚本，脚本中填入`tor -f torrc`，以后启动Tor直接运行这个脚本就可以啦。

## Linux下使用Tor

Debian/Ubuntu下，可直接安装

```sh
sudo apt install tor -y
```

修改配置文件`/etc/tor/torrc`,在其末尾添加如下内容

```
# 设置前置socks5代理
Socks5Proxy 127.0.0.1:1080
# 排除有安全隐患的国家
ExcludeNodes {cn},{hk},{mo},{kp},{ir},{sy},{pk},{cu},{vn},{ru}
ExcludeExitNodes {cn},{hk},{mo},{sg},{th},{pk},{by},{ru},{ir},{vn},{ph},{my},{cu}
StrictNodes 1
```

启动Tor并设置开机自启动

```sh
systemctl start tor
systemctl enable tor
```

验证tor是否链接成功（安装tor的同时一般会默认附带安装了torsocks）

```bash
torsocks curl myip.ipip.net
```

如果返回tor节点ip或者被Cloudflare拦截返回一段验证html，则说明tor设置成功。

## Tor进阶玩法

### 三重代理

直接使用Tor容易被ISP识别，因为Tor的流量特征太明显了，而加一层前置代理虽然可以解决这个问题，但是被访问的目标网站还是知道你在用Tor，而且有些目标网站对Tor不友好，这时候怎么办呢？我们可以用三重代理，这样一来，你的ISP不知道你在用Tor，你访问的目标网站也不知道你在用Tor。具体实现如下：

Web Browser --> Proxy Client 1(1080) --> Tor Client(9050) --> Proxy Client 2(1081) --> Proxy Server 2 --> Tor Server --> Proxy Server 1 --> Internet

其中，Proxy Server 1为最后出口，即经由它直接访问目标网站，最为薄弱；Proxy Server 2作为连接国内到国外的通道，直接决定访问速度；而Tor Server作为中间层，对接Proxy Server 1的入口。那么，可以把网络上公开的免费的公共代理服务器作为Proxy Server 1，而自己的私人代理服务器作为Proxy Server 2，这样一来便可确保安全的同时保证访问速度。

值得注意的是，Proxy Client 2需要支持前置代理才行。