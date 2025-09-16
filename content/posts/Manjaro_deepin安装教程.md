---
title: Manjaro_deepin安装教程
tags: Linux
abbrlink: 45740
date: 2020-02-03 03:19:55
---

Linux作为一款开源的操作系统，对比于闭源的Windows或MacOS，它更加安全也更注重用户隐私，更重要的是它可以免费使用。但是呢，Linux系统的操作界面对新用户来说可不是那么的友好，也正是是因为这点，许多新用户望而却步。今天，推荐一款用户友好型Linux，操作简单，快速上手，包管理功能强大，自带驱动，省心省力，没错它就是manjaro_deepin!

<!-- more -->

## 一、安装部分

1. 用rufus（或类似的镜像刻录工具）将安装镜像刻录到u盘中。
2. 重启电脑进入WinPE环境（推荐WEPE，纯净），用傲梅分区助手划分一块分区给要安装的Linux（建议100G），然后擦除该分区，即使该分区处于空闲状态。
3. 重启电脑并不断地按F12（联想是按F12,其他机型自己上网查）进入引导启动选择界面，选择U盘引导启动。
4. 进入manjaro引导设置界面，按Enter键默认English语言进入，（不要更换中文，以防出现乱码），点击Manjaro Installer，时区选Shanghai，其他默认，到安装分区这一步时选择你刚擦除好的那块硬盘的对应的分区，选择Erase disk选项，下一步，设置好用户名、登录密码、root密码，等待安装进度条跑完，重启。

## 二、 配置部分

1. 先删除用不到或不想用的软件，比如hexchat、manjaro-hello等。`sudo pacman -Rs hexchat manjaro-hello firefox`

2. 更换国内镜像源。可选多个，建议选用清华源`sudo pacman-mirrors -i -c China -m rank`

3. 添加archlinuxcn源。用编辑器打开/etc/pacman.conf `sudo nano /etc/pacman.conf `,在文末添加

   ```sh
   [archlinuxcn]
   Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
   ```

   然后按Ctrl+s保存，按Ctrl+x离开。

4. 更新镜像源`sudo pacman -Sy`,安装密archlinuxcn密钥验证`sudo pacman -S archlinuxcn-keyring`,全面升级系统`sudo pacman -Syu`

5. 安装常用软件。如`sudo pacman -S chrome tor-browser vlc wps-office typora vim ark yay aria2 uget electron-ssr bookworm deepin.com.qq.office deepin.com.wechat2`

6. 配置中文环境。安装输入法和中文字体

   ```sh
   ##安装输入法
   yay -S fcitx fcitx.im fcitx-configtool fcitx-googlepinyin
   ##配置输入法
   vim .xprofile 
   ##按 i ，然后在里面加入下面的三行，按Esc，输入 :wq
   export GTK_IM_MODULE=fcitx
   export QT_IM_MODULE=fcitx
   export XMODIFIERS="@im=fcitx"
   ##安装中文字体
   sudo pacman -S ttf-roboto noto-fonts ttf-dejavu
   sudo pacman -S wqy-bitmapfont wqy-microhei wqy-microhei-lite wqy-zenhei
   sudo pacman -S noto-fonts-cjk adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts
   ##配置字体
   vim .config/fontconfig/fonts.conf 
   ##添加下面的内容，保存退出（i，Esc，：wq）
   <fontconfig>
   
       <its:rules xmlns:its="http://www.w3.org/2005/11/its" version="1.0">
           <its:translateRule translate="no" selector="/fontconfig/*[not(self::description)]"/>
       </its:rules>
   
       <description>Manjaro Font Config</description>
   
       <!-- Font directory list -->
       <dir>/usr/share/fonts</dir>
       <dir>/usr/local/share/fonts</dir>
       <dir prefix="xdg">fonts</dir>
       <dir>~/.fonts</dir> <!-- this line will be removed in the future -->
   
       <!-- 自动微调 微调 抗锯齿 内嵌点阵字体 -->
       <match target="font">
           <edit name="autohint"> <bool>false</bool> </edit>
           <edit name="hinting"> <bool>true</bool> </edit>
           <edit name="antialias"> <bool>true</bool> </edit>
           <edit name="embeddedbitmap" mode="assign"> <bool>false</bool> </edit>
       </match>
   
       <!-- 英文默认字体使用 Roboto 和 Noto Serif ,终端使用 DejaVu Sans Mono. -->
       <match>
           <test qual="any" name="family">
               <string>serif</string>
           </test>
           <edit name="family" mode="prepend" binding="strong">
               <string>Noto Serif</string>
           </edit>
       </match>
       <match target="pattern">
           <test qual="any" name="family">
               <string>sans-serif</string>
           </test>
           <edit name="family" mode="prepend" binding="strong">
               <string>Roboto</string>
           </edit>
       </match>
       <match target="pattern">
           <test qual="any" name="family">
               <string>monospace</string>
           </test>
           <edit name="family" mode="prepend" binding="strong">
               <string>DejaVu Sans Mono</string>
           </edit>
       </match>
   
       <!-- 中文默认字体使用思源宋体,不使用 Noto Sans CJK SC 是因为这个字体会在特定情况下显示片假字. -->
       <match>
           <test name="lang" compare="contains">
               <string>zh</string>
           </test>
           <test name="family">
               <string>serif</string>
           </test>
           <edit name="family" mode="prepend">
               <string>Source Han Serif CN</string>
           </edit>
       </match>
       <match>
           <test name="lang" compare="contains">
               <string>zh</string>
           </test>
           <test name="family">
               <string>sans-serif</string>
           </test>
           <edit name="family" mode="prepend">
               <string>Source Han Sans CN</string>
           </edit>
       </match>
       <match>
           <test name="lang" compare="contains">
               <string>zh</string>
           </test>
           <test name="family">
               <string>monospace</string>
           </test>
           <edit name="family" mode="prepend">
               <string>Noto Sans Mono CJK SC</string>
           </edit>
       </match>
   
       <!-- 把Linux没有的中文字体映射到已有字体，这样当这些字体未安装时会有替代字体 -->
       <match target="pattern">
           <test qual="any" name="family">
               <string>SimHei</string>
           </test>
           <edit name="family" mode="assign" binding="same">
               <string>Source Han Sans CN</string>
           </edit>
       </match>
       <match target="pattern">
           <test qual="any" name="family">
               <string>SimSun</string>
           </test>
           <edit name="family" mode="assign" binding="same">
               <string>Source Han Serif CN</string>
           </edit>
       </match>
       <match target="pattern">
           <test qual="any" name="family">
               <string>SimSun-18030</string>
           </test>
           <edit name="family" mode="assign" binding="same">
               <string>Source Han Serif CN</string>
           </edit>
       </match>
       
       <!-- Load local system customization file -->
       <include ignore_missing="yes">conf.d</include>
       <!-- Font cache directory list -->
       <cachedir>/var/cache/fontconfig</cachedir>
       <cachedir prefix="xdg">fontconfig</cachedir>
       <!-- will be removed in the future -->
       <cachedir>~/.fontconfig</cachedir>
   
       <config>
           <!-- Rescan in every 30s when FcFontSetList is called -->
           <rescan> <int>30</int> </rescan>
       </config>
   
   </fontconfig>
   ```

7. 在deepin控制器中将语言改为中文，标准字体改为思源黑体CN，等宽字体改为DejaVu Sans Mono，再到manjaro setting manager中下载对应语言包。

## 三、美化部分

1. 安装mac风格图标。

   ```bash
   yay -S la-capitaine-icon-theme ##然后在deepin控制器的主题项选中它
   ```

2. 安装oh-my-zsh

   ```sh
   sh -c "$(aria2c -x16 https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   ```

3. 配置zsh主题、语法高亮、自动补全

   ```sh
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   
   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   
   vim .zshrc 
   
   ##修改如下：
   ##ZSH_THEME="ys"
   ##plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
   
   ```

   

## 四、常见问题

1. `sudo pacman -Qqdt`可以查询无用依赖以便清除，但是呢俺清除掉后窗口特效就出问题了，会有边框出现。
2. 启动或关机时间较长，一直在那里显示代码，启动界面出现`failed to start load kernel module`,可通过重新安装deepin-anything解决，`yay -S deepin-anything`安装所有community的，然后重启。
3. 每次开机后都会在登录界面卡一会儿才能输入帐号密码，初步判断是keyboard模块还没有加载完成，目前俺还没有找到解决办法。

参考：

 http://panqiincs.me/2019/06/05/after-installing-manjaro/ 