---
title: Mac Blog迁移
date: 2020/10/27 22:28:29
comments: false
cover: https://i.loli.net/2020/10/27/P6yklAhcmjKevFW.png
coverHeight: 750
coverWidth: 1200
categories:
   - Hexo
tags:
   - Mac
   - Hexo
copyright: '<strong>版权声明：</strong>本文采用 <a href="https://creativecommons.org/licenses/by-nc-sa/3.0/cn/deed.zh" target="_blank">CC BY-NC-SA 3.0 CN</a> 协议进行许可'
---
*Ubuntu对于软件适配性的弱势使得小白一枚的我没法进一步学习，无奈，只能开启双系统模式了，于是Blog的转移成为当务之急，本文记录了此次系统安装和迁移Blog的过程，尽量还原关键过程。*
<!--more-->

# 目录
## [一.前言](#jump1)
## [二.Blog环境文件托管(Ubuntu)](#jump2)
## [三.安装主系统Mac15.5.7](#jump3)
## [四.安装副系统Ubuntu](#jump4)
## [五.Mac上重建Blog前置环境](#jump5)
## [六.再次实现环境文件托管](#jump6)

<hr>

# 正文

## <span id="jump1">一.前言<span>

* 本次搞机时常有点久，光是Mac的安装就花了好几天，还是周末趁不断电和不断网熬通宵搞完的。
* Blog环境文件通关搞事情，theme里面的yml配置文件居然没被托管，只好重新配置，这次打了个包，以备急救，hhh~
* SSH配置一直出错，上了一晚上课才回来搞正确的，应该是权限问题。Mac权限管控较为严格。
## <span id="jump2">二.Blog环境文件托管(Ubuntu)<span>

1. Github创建branch-Hexo
   * 在Blog所在仓库创建branch，取名为Hexo(随意)。建议直接在网站创建，速度快。此时Hexo下文件跟master下相同
   
   * Clone仓库文件(即Blog目录文件)，删除除.git文件外所有文件
   * `git add -A //将文件夹变化提交到缓存区`
   * `git commit -m "文件描述" //提交文件`
   * `git push origin hexo //推送到远端分支，此时hexo分支文件被清空`
     注意：1. .git文件为隐藏文件，Ubuntu下在文件管理器中设置可以显                           示。
2. 将.git文件移动到Blog环境文件夹内
   `git branch -a //查看是否存在分支`
   `git checkout hexo //切换到hexo分支`
   `git branch //查看当前分支`
3. 推送Blog环境文件至远程仓库分支
   `git add . //将文件夹变化项目提交到暂存区`
   `git commit -m "文件描述" //提交文件`
   `git push origin hexo //推送至远程分支hexo`
4. 备份完成，此后每次更新blog前重复第3步即可实现双同步。
*注意:由于某些原因，推送时theme下的yml配置文件并不会被推送，导致下次clone后文件不完整，必须重新配置，建议将已配置好的主题文件打包，并将打包文件放置在blog配置文件夹中。血的教训，不解释…猜测可能是theme下的主题自带.git文件和两个.config.yml冲突导致。
## <span id="jump3">三.安装主系统Mac<span>
1.Ubuntu环境下下载[balena](https://www.balena.io/etcher/)，刻录下载好的Mac镜像，可以从[黑果小兵](https://blog.daliansky.net/)这里下载（现在作者最新的开始进行付费模式，但没必要，不过可以支持一下的。)
2. 建议还是另备一台*win10*，随时更改，建议还是用clover吧，oc这次把我坑惨了，不专业的不要搞，从网上找自己对应机型efi替换，安装即可。
3. 这里我卡了四天多，主要是最开始用oc引导，写入主板了，之后安装好后显示oc引导，但是我是clover，clover配置无法生效，试过好几种方法，bios恢复出厂设置；nvram无法清除等，最后机智地行险招，重回win10，在MyAsus(华硕专门工具)更新bios，终于清除了。
4. 这里提一下：若要装双系统的话，先提前分好两个盘的大小，我两个差不多。
5.Mac安装完成，目前亮度无法保存，触摸板无法使用，睡眠睡死，蓝牙链接手机失效，连接耳机可以。
6.安装进入后没法联网，oneplus的文件传输不适合mac高版本，可以使用有线连接，或者使用iphone的usb分享网络，这里感谢室友的iphone6救我于危难之中~
7.可以下载*andriod*文件传输方便mac和手机互通。
8.Github大佬已经研究出mac驱动intel网卡的方法了，不需要专门购买黑苹果网卡驱动了(哭...以前装mac时买了），[项目地址](https://github.com/OpenIntelWireless/itlwm)，按指示操作即可，注意这只是一个驱动，需要下载对应[app](https://github.com/OpenIntelWireless/HeliPort)
## <span id="jump4">四.安装Ubuntu复系统<span>
1. 主要问题可以参见[我的另一篇文章](https://www.jianshu.com/p/26216a04a73b)

2. Ubuntu20.10相比20.04变化有些大，所以部分方法不适用。

3. 注意安装时分盘选择其它选项，将剩下的硬盘空间分成四份，分别挂载:

   * /   分配20-40G  用于分配主目录下所有未被指定分配的目录

   * /boot   内核 1G+

   * /home 用户目录，尽量大

   * /tmp 5G左右

## <span id="jump5">Mac上重建Blog前置环境<span>
0. Mac上配置出问题主要在npm和ssh上。
1. 安装brew——一个Mac上的高效包管理器(也可以在浏览器下载nodejs)
    `sudo mkdir /usr/local/Homebrew //创建Homebrew目录`
    `sudo git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git /usr/local/Homebrew  //同步清华brew.git库`
    `sudo ln -s /usr/local/Homebrew/bin/brew /usr/local/bin/brew  //添加环境变量`
    `sudo mkdir -p /usr/local/Homecore/Library/Taps/homebrew/homebrew-core`
    `sudo git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core //上述两步同步core库`
    `sudo mkdir -p /usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask`
`sudo git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git /usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask //同步cask`
`sudo chown -R $(whoami) /usr/local/Cellar //提权`
`brew -v //查看版本号`
`cd "$(brew --repo)" `
`git remote set-url origin git://mirrors.ustc.edu.cn/brew.git 、、替换brew默认源`
`brew update //更新`

*上述brew教程来自于[huanhao](https://www.cnblogs.com/huanhao/p/installbrew.html)*
2. 安装nodejs
`brew install node`
3.使用[cnpm](https://developer.aliyun.com/mirror/NPM?from=tnpm)代替npm管理nodejs
    `$ npm install -g cnpm --registry=https://registry.npm.taobao.org
`
4. 安装hexo
    `cnpm install -g hexo-cli //安装hexo`
5. 创建Blog文件夹，并用`hexo init`初始化
6. 配置git代理
    `git config --global https.proxy socks5://127.0.0.1:port   //port为代理socks协议的本地监听端口,我配置http不成功`
    `lsof -n -P -i  //查询Mac端口`
    `git config --global -e //查看代理配置`
    `git config --global --unset https.proxy //取消对应代理`
7. `git clone -b hexo url //在桌面从仓库地址拉取环境文件所在分支`
    *注意：git的速度很慢，建议使用代理，部分代理依然不支持Github代理加速。*
8. 根据环境文件的packge.json文件安装对应hexo插件，使用cnpm安装即可。
    *注意：npm中的hexo-depolyer-git已经不支持了，会出现很多问题，建议用cnpm安装*
9.  将之前clone好的文件中_config.yml替换hexo初始化的同名文件，souce文件，theme文件也要进行替换(包含所有文章)以及其它原来做过改动的。
10. 配置ssh
    *配置这个花我老长时间了，一直显示`git@github.com: Permission denied (publickey).`*
1. `sudo mkdir Blog //在文稿中创建Blog文件(关键)`
2. `sudo ssh-keygen -t rsa -C "youremail" //在新建的文件夹生成ssh秘钥，此时不会保存在根目录。`
3. `sudo vim /~/id_rsa.pub //注意前面的~这里是为了指代后面文件的具体位置，因人而异，当然你也可以直接打开该文件，复制即可`
4.配置git
    `git config --global user.name "yourgiuthubname" `
    `git config --global user.enail "youremail"`
5. `sudo ssh -T git@github.com //验证登录，success！！！`
## <span id="jump6">再次实现环境文件托管<span>
*这个就不用讲了，.git文件已经存在，移动到Blog文件夹即可，步骤跟前面一样。*
*注意：mac默认会隐藏部分文件夹，且只能通过命令行才能解除
`defaults write com.apple.finder AppleShowAllFiles -boolean true;killall Finder  //显示隐藏文件`
`defaults write com.apple.finder AppleShowAllFiles -boolean false;killall Finder //隐藏隐藏文件`
建议使用完后还是恢复隐藏属性*


*hu~\(^o^)/~终于写完了……每一次记录都是一次进步，*