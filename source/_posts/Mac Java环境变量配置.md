---
title: Mac Java环境变量配置
date: 2020/10/29 17:08:52
comments: false
cover: https://i.loli.net/2020/10/29/CnS9qr2btcygmGB.png
coverHeight: 750
coverWidth: 1200
categories:
   - Mac
tags:
   - Mac
   - Problems
copyright: '<strong>版权声明：</strong>本文采用 <a href="https://creativecommons.org/licenses/by-nc-sa/3.0/cn/deed.zh" target="_blank">CC BY-NC-SA 3.0 CN</a> 协议进行许可'
---

***配置通过IDEA下载的OpenJDK的Java环境，实现vscode编写Java***

<!--more-->

####  步骤：

* `cd ~  //终端输入，进入用户目录`

* IDEA(或者甲骨文自家的)下载的默认安装路径：/Listary/Java/JavaVirtualMachines/

  *注意：默认JDK文件会隐藏，可以终端输入命令显示隐藏文件，见“Mac 迁移 Blog”此文*

* `touch .bash_profile  //新建该文件，如果之前配置过brew换过源则已经存在，存在该文件则不会新建 `

* `vim .bash_profile //编辑该文件`

* `JAVA_HOME='/Library/Java/JavaVirtualMachines/openjdk-15.0.1.jdk/Contents/Home'`

  `export JAVA_HOME`

  `export PATH=$JAVA_HOME/bin:$PATH`

  *添加上面三行代码,openjdk-15.0.1.jdk为文件名，其中后缀名一般隐藏*
  
* `source .bash_profile //使更改生效`

*  `java --version  //查看java环境是否更改成功(mac自带java环境)`