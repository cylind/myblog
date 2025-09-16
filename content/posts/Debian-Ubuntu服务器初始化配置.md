---
title: Debian&Ubuntu服务器初始化配置
tags: Linux
abbrlink: 19842
date: 2020-10-13 12:55:25
---

本文以Debian 10为例，介绍如何对服务器进行初步配置，本文同样完全适用于 Ubuntu20.04系统。

<!-- more -->

## 基础设置

更改密码:

```bash
passwd
```

更新系统:

```bash
apt update && apt full-upgrade -y && apt autoremove -y
```

安装常用软件：

```bash
apt install sudo wget curl htop vim git ffmpeg aria2 zsh zip unzip man npm nodejs speedtest-cli python3 python3-pip ufw fail2ban certbot nginx -y
```

安装python3常用库：

```bash
pip3 install requests bs4 lxml python-telegram-bot you-get youtube-dl gdown m3u8downloader fake_useragent faker pyexecjs
```

添加新用户：

```bash
adduser chan
```

将新用户添加至sudo组：

```bash
usermod -aG sudo chan
```

## 安全设置

### ssh设置

修改ssh配置文件

```bash
vim /etc/ssh/sshd_config
```

修改ssh端口：

```bash
Port 2333
```

禁止root远程登录：

```bash
PermitRootLogin no
```

安装fail2ban防ssh爆破：

```bash
cd /etc/fail2ban
cp jail.conf jail.local
nano jail.local
```

sshd处加上 enabled = true，并将所有默认ssh端口为2333

### 防火墙设置

开启防火墙，仅放行ssh端口

```bash
ufw allow 2333
ufw enable
```

## 性能优化

### TCP传输优化

查看是否已开启BBR：

```
sysctl net.ipv4.tcp_available_congestion_control
```

如果没有则开启BBR：

```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

### 调整swap大小

```
wget https://www.moerats.com/usr/shell/swap.sh && bash swap.sh
```

## 个性化配置

### 配置shell

安装oh-my-zsh

```
wget https://cdn.jsdelivr.net/gh/ohmyzsh/ohmyzsh/tools/install.sh && bash install.sh
```

添加自动补全插件：

```
git clone https://github.com.cnpmjs.org/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

添加语法高亮插件：

```
git clone https://github.com.cnpmjs.org/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

修改配置文件`~/.zshrc`,修改主题及插件：

```
ZSH_THEME="ys"
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

### 配置vim

#### 安装spf13-vim

```bash
sh <(curl https://j.mp/spf13-vim3 -L)
```

#### 启用鼠标复制粘贴

编辑`/etc/vim/vimrc.local`，添加如下内容

```
source /usr/share/vim/vim81/defaults.vim
let skip_defaults_vim = 1
if has('mouse')
    set mouse-=a
endif
```

#### F5执行脚本

将下面的配置放到.vimrc文件即可：

```
""""""""""""""""""""""
"Quickly Run
""""""""""""""""""""""
map <F5> :call CompileRunGcc()<CR>
func! CompileRunGcc()
    exec "w"
    if &filetype == 'c'
        exec "!g++ % -o %<"
        exec "!time ./%<"
    elseif &filetype == 'cpp'
        exec "!g++ % -o %<"
        exec "!time ./%<"
    elseif &filetype == 'java'
        exec "!javac %"
        exec "!time java %<"
    elseif &filetype == 'sh'
        :!time bash %
    elseif &filetype == 'python'
        exec "!time python2.7 %"
    elseif &filetype == 'html'
        exec "!firefox % &"
    elseif &filetype == 'go'
        exec "!go build %<"
        exec "!time go run %"
    elseif &filetype == 'mkd'
        exec "!~/.vim/markdown.pl % > %.html &"
        exec "!firefox %.html &"
    endif
endfunc
```

## 参考

https://blog.chaos.run/dreams/ubuntu-server-starting-settings

https://github.com/fail2ban/fail2ban/wiki/Proper-fail2ban-configuration

https://xirikm.net/2019/504-1