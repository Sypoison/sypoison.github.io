---
title: Mac+Ubuntu双端
date: 2020/11/ 22:07:21
comments: false
cover: https://i.loli.net/2020/10/29/CnS9qr2btcygmGB.png
coverHeight: 750
coverWidth: 1200
categories:
   - Mac
   - Ubuntu
tags:
   - zsh
copyright: '<strong>版权声明：</strong>本文采用 <a href="https://creativecommons.org/licenses/by-nc-sa/3.0/cn/deed.zh" target="_blank">CC BY-NC-SA 3.0 CN</a> 协议进行许可'
---

***bash糟糕的体验实在是受不了，特别是设置代理和拓展性方面，于是采用zsh，并将其个性化，最后只能说舒服极了！***

<!--more-->

# 目录

### 1.安装zsh(此前已安装git)

`sudo apt install zsh `

### 2.zsh 替换 bash

`chsh -s /bin/zsh //相反，chsh -s /bin/bash为替换为bash`

`shutdown -r 0 //立即重启`

### 3.终端调整背景透明

***直接在终端首选项调整即可。***

### 4.下载oh-my-zsh

***官网提供的方法：国内没法访问，zsh按照网上设置代理也没用，建议直接复制地址到浏览器地址栏，直接下载该文件(注意开魔法)，然后运行下面命令：***

`sudo sh install.sh //也可以保留该脚本，以后备用`

***手动安装：从Gorhub原仓库下载，命令如下：***

`git clone https://github.com/robbyrussell/oh-my-zsh //克隆原仓库，权限不够的话加sudo`

`cd oh-my-zsh/tools //进入脚本所在目录`

`sh install.sh //运行脚本`

* 注意：mac安装时，会出现报错，对于提示中的文件目录依次赋予权限：

  `sudo chmod 775 folder`

### 5.安装字体

`sudo apt install powerline fonts-powerline`

### 6.主题默认，觉得可以了，无需更改

### 7.安装autojump

***耗时较久，但双端差不多，该插件可用于快捷跳转，使用命令 jc url (终端进入) jo url (在文件管理器中打开)***

[介绍](https://www.linuxidc.com/Linux/2015-08/121421.htm)

* 注意：autojump作用是通过记忆操作实现快捷进入，因此使用jc快速进入目录时应该使用cd进入目录，以后可以凭借关键字进入。

***对于Mac，使用brew，对于ubuntu，使用apt安装***

`brew install autojump //mac`

`sudo apt install autojump //ubuntu`

`vim ~/.zshrc //打开后，在plugins的括号里面添加autojump，最后一行添加下列代码：`

`[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && . ~/.autojump/etc/profile.d/autojump.sh //保存退出。重启终端。`

### 8.启用高亮显示

`sudo apt install zsh-syntax-highlighting //mac下有所不同`

### 9.安装快捷代理设置

`git clone https://github.com/sukkaw/zsh-proxy.git ~/.oh-my-zsh/custom/plugins/zsh-proxy # 编辑 ~/.zshrc ，找到 plugins 配置，添加 zsh-proxy source ~/.zshrc # 看到屏幕输出 init_proxy 的字样表示安装成功 init_proxy config_proxy # 根据屏幕提示依次录入 socks5 和 http 代理地址 proxy`(来源网络)

`proxy //启动`

`noproxy //断开`

* 是不是很方便？今后都可以这样直接打开了，同时会代理适配zsh环境的工具，比如git等。

* 注意：只需要配置socks5即可，http可以不用配，如果更换端口，则再次运行下列命令：

  `init_proxy `

  `config_proxy`

  重新配置即可。

  平时不用最好关掉。

### 10.安装自动补全incr插件

[下载地址](https://mimosa-pudica.net/zsh-incremental.html)

***在.oh-my-zsh文件plugins下新建incr，移动下载文件到目录下，运行下列命令：***

`source incr*.zsh`

