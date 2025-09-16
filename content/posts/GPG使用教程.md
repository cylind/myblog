---
title: GPG使用教程
date: 2021-01-30 17:02:00
tags: [GPG]
---

**GNU Privacy Guard**（**GnuPG**或**GPG**）是一个密码学软件，用于加密、签名通信内容及管理非对称密码学的密钥。其主要特点是非对称加密，即公钥加密，私钥解密；其主要应用于不安全的环境下加密通讯。

<!-- more -->

### 背景知识

[PGP - 维基百科](https://zh.wikipedia.org/wiki/PGP)

[GnuPG - 维基百科](https://zh.wikipedia.org/wiki/GnuPG)

[OpenPGP, PGP, and GPG: What is the difference?](https://www.goanywhere.com/blog/2013/07/18/openpgp-pgp-gpg-difference)

### 安装

不太熟悉命令行操作的可以使用`seahorse`作为GUI管理工具，Linux Mint自带该工具，方便平时管理。但它有个缺点导入密钥貌似有bug，导入不了，得出命令行下导入。

```bash
$ sudo apt update && sudo apt install gpg seahorse -y
```

如果你用的是Linux Mint的话，可以添加个文件管理插件nemo-seahorse，可以很方便的进行加解密，签名等操作；如果你用的是Ubuntu的话，可以安装seahorse-nautilus，一样的功能。

### 生成密钥对

这一步可以在seahorse提供的GUI界面进行，很方便的。打开界面，点击`+`号，选择添加GPG密钥，填写名称和邮箱，加密方式选RSA，密钥长度设置为4096，过期时间设置为永不过期，点击生成。

### 校验文件

网站一般会提供源文件和证书文件(.asc)，下面假设文件为file.ext，签名证书为file.ext.asc，文件发布者公钥为signing-key.asc，key id 为3DBDC284

首先，下载文件拥有者的公钥，然后使用 gpg 命令导入公钥到keyring 中

```bash
gpg --import signing-key.asc
```

如果，没下载有拥有者的公钥，那么，先执行

```bash
gpg --verify file.ext.asc
```

这时候会得到一个key id，搜索并导入这个key

```bash
gpg --search-keys 3DBDC284
```

其次，验证公钥

```bash
gpg --fingerprint 3DBDC284
```

3DBDC284是导入公钥时得到的Key ID，每个Key都不同。

```bash
gpg --verify file.ext.asc file.ext
```

### 数据备份

查看当前私钥，并决定备份那一个

```bash
$ gpg --list-secret-keys --keyid-format LONG
/home/chan/.gnupg/pubring.kbx
-----------------------------
sec   rsa4096/22D0F3ED7D6E2D2D 2021-01-28 [SC]
      7363D87748565E3109B2DEDF22D0F3ED7D6E2D2D
uid                 [ 绝对 ] yulinchan <chenyulin775@gmail.com>
ssb   rsa4096/46314546EE607336 2021-01-28 [E]
```

导出GPG私钥（私钥已经包含公钥）

```bash
$ gpg -o private.gpg --export-options backup --export-secret-keys chenyulin775@gmail.com
```

执行该命令后，在当前目录下生成private.gpg备份私钥。该命令中的`--export-options backup`参数，可以确保导出恢复GPG密钥所需的所有数据。

如果你还设置有口令保护私钥的话，还会弹出一个提示窗口让你输入口令，才能完成导出。导出完成后，即可把private.gpg放到一个安全的地方保存起来。

### 数据恢复

导入私钥

```bash
$ gpg --import-options restore --import private.gpg
```

执行该命令后，GPG将从当前目录下的private.gpg导入密钥（这里的私钥已经包含公钥）。参数`--import-options restore`确保GPG完整恢复密钥。

同样的，如果私钥设置有保护口令的话，还会弹出一个提示窗口让你输入口令，才能完成导入。导入密钥后还要设置密钥的信任状态

```bash
$ gpg --edit-key chris@seagul.co.uk trust quit
```

参考：

https://www.ruanyifeng.com/blog/2013/07/gpg.html

https://blog.chaos.run/dreams/using-gpg

https://gist.github.com/chrisroos/1205934

https://www.jwillikers.com/backup-and-restore-a-gpg-key

https://serverfault.com/questions/86048/gpg-what-do-i-need-to-backup

https://blog.chaos.run/dreams/gpg-verify/

https://www.cnblogs.com/shenfeng/p/gpgverify.html