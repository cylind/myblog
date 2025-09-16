---
title: 使用pyinstaller打包py脚本成exe
tags: Python
abbrlink: 53573
date: 2020-08-15 13:30:37
---

## python相关文件

1. py文件 : 源码文件，运行需要使用者安装Python环境并且安装依赖的各种库。
2. pyc文件：pyc文件是Python解释器可以识别的二进制码，可跨平台的，需要使用者安装相应版本的Python和依赖库。
3. 可执行文件：不需要安装python环境和依赖库,可针对不同平台需要打包不同的可执行文件（Windows,Linux,Mac,...）

<!-- more -->

## pyinstaller原理

1. PyInstaller工具可以把python解析器和脚本打包成一个可执行的文件，并不是编译成真正的机器码，打包成一个可执行文件后运行效率可能会降低，好处就是在使用者的机器上可以不用安装python和你的脚本依赖的库。
2. 利用PyInstaller对指定的的脚本打包时，会先分析脚本所依赖的其他脚本，然后根据导包路径去查找，把所有相关的脚本收集起来，包括Python解析器，然后根据你的命令参数可分别生成文件夹，或者打包成一个可执行文件。
3. 无论是生成的文件夹里的可执行文件或者只打包成一个可执行文件都可以直接运行，前者需要把整个文件夹都给别人。

## pyinstaller安装

Windows下，打开命令行，执行pip install PyInstaller命令即可。在windows下，pyinstaller需要PyWin32的支持，当用pip安装PyInstaller时未找到PyWin32，会自动安装pypiwin32。

## pyinstaller打包

```
pyinstaller -F test.py
```

-F 指只生成一个exe文件，不生成其他dll文件
-w 不弹出交互窗口,如果你想程序运行的时候，与程序进行交互，则不加该参数
-i 设定程序图标 ，其后面的xxx.ico文件就是程序小图标
-p 添加二进制文件查找目录。比如upx压缩程序所在目录。
–hidden-import=pandas._libs.tslibs.timedeltas 隐藏相关模块的引用

将upx.exe放入pyinstaller.exe所在的同级目录（一般是python安装目录下的script文件夹），可不用-p参数指定upx.exe所在位置。附upx程序下载地址：https://github.com/upx/upx/releases



如果报错，则不要使用upx压缩 pyinstaller --noupx --onedir --onefile

参考：

https://stackoverflow.com/questions/38811966/error-when-creating-executable-file-with-pyinstaller

https://pyinstaller.readthedocs.io/en/stable/index.html