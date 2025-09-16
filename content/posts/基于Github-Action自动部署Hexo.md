---
title: 基于Github Action自动部署Hexo
tags: Hexo
abbrlink: 873
date: 2021-02-04 16:45:51
---



老早就听闻Hexo + Github Action这操作，源文件备份，自动化构建，省时省力，真是羡煞我也，但是一直对Git操作和Github Action的写法不熟悉，弄不来。现在总算有时间学一遍Git的操作了，学完刚好来实践一番,顺便把过程记录一下。

<!-- more -->

## 准备工作

### 本地Hexo博客正常

首先，确认 `_config.yml` 文件中有类似如下的 `GitHub Pages` 配置：

```yaml
deploy:
  type: git
  repository: git@github.com:YulinChan/YulinChan.github.io.git
  branch: master
```

其次，确保Hexo能在本地正确运行且可以部署到Github Page。

### 创建所需的仓库

#### 远程Github仓库

1. 创建 `blog` 仓库用来存放 Hexo 项目
2. 创建 `your.github.io` 仓库用来存放静态博客页面

#### 本地Git仓库

进入在本地Hexo博客文件夹下，先去除一些不必要的文件和文件夹，如`.deploy_git`，因为这些文件在部署的时候都会重新生成一遍，所以没必要保留。 以后在本地调试只需要执行`hexo s`即可，没有必要在本地生成html。

```shell
hexo clean
rm -rf .deploy_git
```

另外，值得注意的是，如果Hexo的主题是通过`git clone`获取的（即主题文件夹也是一个git仓库），会出现git仓库嵌套，那么在部署Github Action时就要使用特定actions/checkout来完整签出仓库内容，否则在自动部署时会找不到主题模板。当然了，还有一个比较简单粗暴的方法，删除主题文件下Git仓库相关文件及文件夹，如`.git` ，我当然是用的删除法啦。

然后，执行如下命令，初始化本地Git仓库并与Github上的远程仓库想关联。

```shell
git init
git add .
git commit -m "init git"
git remote add origin git@github.com:YulinChan/blog.git
git push -u origin main
```

### 生成部署密钥

执行如下命令来生存ssh密钥对

```shell
ssh-keygen -t rsa -b 4096 -C "Hexo Deploy Key" -f github-deploy-key -N ""
```

当前目录下会有 `github-deploy-key` 私钥 和 `github-deploy-key.pub`  公钥两个文件。

### 配置部署密钥

#### blog仓库上添加私钥

复制 `github-deploy-key` 文件内容，在 `blog` 仓库 `Settings -> Secrets -> Add a new secret` 页面上添加。

1. 在 `Name` 输入框填写 `HEXO_DEPLOY_KEY`。
2. 在 `Value` 输入框填写 `github-deploy-key` 文件内容。

#### your.github.io仓库上添加公钥

复制 `github-deploy-key.pub` 文件内容，在 `your.github.io` 仓库 `Settings -> Deploy keys -> Add deploy key` 页面上添加。

1. 在 `Title` 输入框填写 `HEXO_DEPLOY_PUB`。
2. 在 `Key` 输入框填写 `github-deploy-key.pub` 文件内容。
3. 勾选 `Allow write access` 选项。

## 创建Github Action

在 `blog` 仓库根目录下创建 `.github/workflows/deploy.yml` 文件，目录结构如下。

```shell
blog (repository)
└── .github
    └── workflows
        └── deploy.yml
```

在 `deploy.yml` 文件中填以下内容。

```yaml
name: Hexo Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    if: github.event.repository.owner.id == github.event.sender.id

    steps:
      - name: Checkout source
        uses: actions/checkout@v2
        with:
          ref: main

      - name: Setup Node.js
        uses: actions/setup-node@v1
        with:
          node-version: '12'

      - name: Setup Hexo
        env:
          ACTION_DEPLOY_KEY: ${{ secrets.HEXO_DEPLOY_KEY }}
        run: |
          mkdir -p ~/.ssh/
          echo "$ACTION_DEPLOY_KEY" > ~/.ssh/id_rsa
          chmod 700 ~/.ssh
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan github.com >> ~/.ssh/known_hosts
          git config --global user.email "chenyulin@yandex.com"
          git config --global user.name "YulinChan"
          npm install hexo-cli -g
          npm install

      - name: Deploy
        run: |
          hexo clean
          hexo deploy
```

上面的email和name要根据实际情况修改。

设置好上面的配置文件后，要先将其更新到本地Git仓库，不然会因为冲突而无法push，因为远程仓库添加了新文件。更新只需在本地Hexo博客文件夹执行pull更新即可

```shell
git pull
```

然后，随便生成一篇新文章并push到Github上

```shell
hexo new my_new_post
git add .
git commit -m "new post"
git push
```

最后，可以到Github仓库的Actions里查看情况了，如果是绿色的勾则代表成功了。

## 参考

https://tommy.net.cn/2020/08/06/deploy-hexo-with-github-actions/

https://sanonz.github.io/2020/deploy-a-hexo-blog-from-github-actions

