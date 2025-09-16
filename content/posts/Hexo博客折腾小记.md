---
title: Hexo博客折腾小记
abbrlink: 52996
date: 2020-10-08 13:42:22
tags: Hexo
---

## Hexo迁移到Linux

首先，要在Github上设置有本地Linux的SSH公钥。具体设置过程见Github官方教程，大体操作如下,先生成SSH密钥对

```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
$ eval "$(ssh-agent -s)"
> ssh-add ~/.ssh/id_ed25519
```

并将生成的公钥复制保存到Github的SSH密钥设置里

```bash
$ sudo apt-get install xclip
# Downloads and installs xclip. If you don't have `apt-get`, you might need to use another installer (like `yum`)

$ xclip -selection clipboard < ~/.ssh/id_ed25519.pub
# Copies the contents of the id_ed25519.pub file to your clipboard
```

其次，还要在Linux上安装nodejs, npm和hexo-cli。用apt默认安装的nodejs版本较低，并不能满足Hexo-cli的依赖包，所以要添加额外源进行安装（注意，要安装12.x的，14.x部署到github会报错）

```sh
curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -
```

安装完nodejs后还要安装npm和hexo-cli

```sh
sudo apt install npm -y
sudo npm install hexo-cli
```

最后，将Hexo文件夹复制（或移动）到Linux操作系统可访问的文件夹下，并在该文件夹下执行如下命令即可

```bash
npm install
```

## 主题升级/更换

1. 下载最新的主题：

```sh
git clone https://github.com/next-theme/hexo-theme-next
```

2. 修改主题文件夹下的配置文件：
   利用文本比对工具，比对新旧主题文件夹下的_config.yml配置文件，并按需修改。
   复制旧主题文件夹的source/images下的图像到新主题对应的文件夹下。
   修改新主题文件夹的languages下的翻译文件zh-CN.yml: 添加对应的中文翻译。

3. 修改Hexo的_config.yml配置文件，切换主题为新下载的主题

   ```yaml
   theme: next8.0.1
   ```

4. 本地调试

   ```bash
   hexo g && hexo s
   ```

## DIY主题布局与样式

主题的布局结构文件放在layout文件夹下；而主题的样式文件放在source/css下，其中的_variables里存放着各中css样式的参数设定，主要修改这里即可。

一般要配合开发者调试工具进行样式修改，可先定位css的类名，再在主题文件夹下查找目标类名字符串

```bash
grep -rn "class_name" *
```

## 参考：

[Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

[Hexo版本升级和Next主题升级之坑](https://whjkm.github.io/2018/07/17/Hexo%E7%89%88%E6%9C%AC%E5%8D%87%E7%BA%A7%E5%92%8CNext%E4%B8%BB%E9%A2%98%E5%8D%87%E7%BA%A7%E4%B9%8B%E5%9D%91/)

