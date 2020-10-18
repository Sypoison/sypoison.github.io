---
title: Ubuntu构建Blog总结
date: 2020/10/14  22:28:10
commments: ture
cover:  https://i.loli.net/2020/10/14/hU9Yj1yWGwkfHrP.png
coverWidth: 1200
coverHeight: 750
copyright: '<strong>版权声明：</strong>本文采用 <a href="https://creativecommons.org/licenses/by-nc-sa/3.0/cn/deed.zh" target="_blank">CC BY-NC-SA 3.0 CN</a> 协议进行许可'
---

*通过Ubuntu自带bash和Github可以快速搭建Blog，本文全程介绍了我在搭建Blog时的操作和遇到的问题和纠正，旨在记录并备份，解决方法大部分来自论坛和官网。*

<!--more-->

# 目录

## [一.前言](#jump1)

## [二.准备工作](#jump2)

## [三.搭建Blog](#jump3)

## [四.更换主题和配置](#jump4)

## [五.关联域名](#jump5)

## [六.更新Blog和备份环境文件](#jump6)

## [七.推荐内容](#jump7)

<hr>
# 正文

## <span id="jump1">一.前言<span>


* 开始使用*Wordpress*建站，非常方便，可是在未买云服务器时没法实现外部访问，于是采用跟*Windows*下相同方法利用*Github*和*hexo*建立个人博客，采用模板为[nexmoe](https://docs.nexmoe.com/hexo-nexmoe/start)。
* *Ubuntu*使用命令行创建速度显然快于*Windows*下
* 使用hexo建站过程出现过几处由于Github帐户设置和本地文件设置的问题进行了归档 处理。
* 所有命令大都需要在root环境运行，因此建议提前进入root模式或者每行命令加sudo

## <span id="jump2">二.准备工作<span>

1. [Freenom](https://www.freenom.com/zh/index.html?lang=zh)申请域名(注意免费续期)

   ![image](https://i.loli.net/2020/10/15/OGNWUrA6bjL9mKM.png)

   注意：

   * Google登录时地址必须在美国。
   * 不能直接结算，必须先修改个人信息，地址必须填美国ip对应城市，州，邮编，具体地址，电话号码等(部分可以乱填)。
   * 关闭浏览器广告插件，以免显示不正确。

2. 注册[Github](https://github.com/)

   * 创建新repository，*将仓库名称设置为 yourname.github.io*

   * 在浏览器地址栏输入仓库名，不报错代表成功。

   * 设置邮箱为可见Email-->Block command line pushs …取消打勾。

     解决error: 推送一些引用到 'git@github.com:xxxx.github.io.git' 失败

3. 配置git(*注意：所有命令需要在root环境运行，即每个命令加sudo*)
`$ apt install git// 安装git`
`$ git --version//检测git版本`
`$ git config --global user.name "yourname" //设置git目标用户(github帐户名)`
`$ git config --global user.email "youremail" //设置git目标邮箱(github帐户注册邮箱)`

4. 配置nodejs(*注意：国内速度可能很感人*)

   `$ sudo apt install nodejs  //安装nodejs(国内可能速度很慢)`
   `$ sudo apt install npm  //安装nodejs的包管理工具`
   `$ sudo npm install npm -g //升级新版npm`
   `$ sudo npm install hexo-cli -g //安装hexo` 
   `$ sudo npm install --save hexo-deployer-git //解决ERROR Deployer not    found:git`

## <span id="jump3">三.搭建Blog<span>
1. 本地网页设置
      *注意：*所有的hexo命令必须在Blog文件下打开才有效。
   `$ sudo mkdir /home/yourname(即计算机用户名)/Blog    /*新建文件夹Blog(区别touch和mkdir)*/`
   `$ sudo hexo init url /*url指代上面创建的文件。*/`
   `$ sudo hexo g   /*生成静态网页。*/`
   `$ sudo hexo s  /*启动本地服务器，可以在浏览器打开http://localhost:4000/。注意如果启动失败，显示启动//失败，则输入下面命令：*/`
   `$ sudo hexo s -p 8000  /*8000指代端口，可以随意填写，应付4000被占用的情况*/`
   `$ sudo ssh-keygen -t rsa -C "youremail"  /*生成ssh公钥和私钥，提示文件存储位置，直接默认。提示输入密码，直接两次回车，不需要密码。打开文件管理，进入目标目录，设置中设置显示隐藏文件，打开id_rsa.pub，复制全部内容，填写到github设置中添加新的ssh公钥，名称随便。*/`
   `$ ssh -T git@github.com  //出现yes/no时选择yes(可能没有此提示)，最后会有Hi…`

2. 配置_config.yml      
     * 注意，下面修改都需要终端修改，也可以终端赋予权限后修改。

     * 修改*author*

        ![img](https://i.loli.net/2020/10/15/zqiam1326THFCXL.png)

     * 修改*url*

        ![img](https://i.loli.net/2020/10/15/JvAFkaMuHbxoe2n.png)

     * 修改Deployment

          ![img](https://i.loli.net/2020/10/15/H34MidBbFYxjefs.png)

     * 上面的repo也可以改为
      ~~~bash
      git@github.com:yourname/yourname.github.io.git
      ~~~

      两种方式都可能生效。
3.推送到repo
    `$ sudo hexo cl //清除缓存`
    `$ hexo g //生成静态网页`
    `$ hexo d //推送到repo，可能出现问题ERROR Deployer not found: git，输入下面一行命令：`
    `$ sudo install --save hexo-deployer-git//此后可能需要重新配置git`
4.地址栏输入yourname.github.io，成功进入则代表成功。此时，你已经有了你的个人Blog。
## <span id="jump4">四.更换主题及配置<span>

* 直接点击[nexmoe](https://docs.nexmoe.com/hexo-nexmoe/start)即可，其它主题类似，你可以在Hexo官网找到更多主题。
* 配置主题需要在theme中的_config.yml和Blog的 _config.yml进行配置，注意备份。
## <span id="jump5">五.关联域名<span>
1. 将申请到的域名在Freenom中更改，添加DNSPOD的解析服务。
2. 获取你的的网站ip：
   ~~~bash
   ping yourname.github.io
   ~~~
3. 注册DNSPOD，注意先实名，然后腾讯可能会给你打电话，直说即可。
4. 在DNSPOD解析服务中添加你的域名和IP
   ![ip](https://i.loli.net/2020/10/15/iutNdgIF7o63Oyw.png)
5.在本地Blog-->source-->_posts里面创建文件CNAME，里面填写你的域名，之后hexo g + d即可。

## <span id="jump6">六.更新Blog和备份环境文件<span>

略，待补充。

## <span id="jump7">七.推荐内容<span>

* [nexmoe](https://docs.nexmoe.com/hexo-nexmoe/start)：很好的主题介绍网站。
* [SM.MS](https://sm.ms/)：很棒的图床网站。
* [Typora](https://www.typora.io/)：三端通用的markdown编写神器。
* [DNSPOD](https://console.dnspod.cn/)：腾讯旗下，提供免费DNS解析服务。
* [Freenom](https://www.freenom.com/zh/index.html?lang=zh)：免费注册域名，到期前一段时间会提示免费续期，过期后只能花钱，因此注意邮箱提         示。
