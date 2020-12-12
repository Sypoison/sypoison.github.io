---
title: Manjaro配置备份
date: 2020/12/12 19:33:29
comments: false
cover: https://i.loli.net/2020/12/13/xUBrYMig6QhvPZa.png
coverHeight: 750
coverWidth: 1200
categories:
   - Manjaro
tags:
   - Manjaro
   - Linux
copyright: '<strong>版权声明：</strong>本文采用 <a href="https://creativecommons.org/licenses/by-nc-sa/3.0/cn/deed.zh" target="_blank">CC BY-NC-SA 3.0 CN</a> 协议进行许可'
---
一切源于手贱，删了Ubuntu之后再也安装不上了，于是选择了对硬件支持更好的Manjaro，前者跟N卡有仇，后者可以直接安装，之后进行了一些问题修复和美化，基本外观与Mac相似，同时附带一些应用安装的解决方案。
<!--more-->
# 目录
## [一.前言：比较Manjaro的三种发行版](#jump1)
## [二.安装：Manjaro xfce](#jump2)
## [三.系统问题与修复](#jump3)
## [四.系统安装后的最初配置](#jump4)
## [五.美化](#jump5)
## [六.尾声](#jump6)



# 正文
## <span id="jump1">一.前言<span>
![成品](https://i.loli.net/2020/12/13/e1aqIbvLpFtcY6u.png)

* 本次先后体验了kde，gnome，xfce版本，感觉各不相同：
	* kde:可定制极高，可是内容太多反而感觉体验不好，太花里胡哨了。
	* gnome:安装失败，无法启动，但从安装见面来看，类似Mac界面，比Ubuntu更美观。
	* xfce:很简单的一个版本，但同样支持基本的个性化定制。
* 最终将kde搞得异常花里胡哨后转战xfce，由于过于简单，出现一些问题，网上无法找到大量使用案例，只能不断尝试。
## <span id="jump2">二.安装<span>
1. 官方下载xfce精简版，会自动从中国最快源下载镜像。
2. 使用rufus写入镜像，默认设置即可。注意：安装过程最好断网。以防更新卡死。
3. U盘启动的的第一个默认界面，驱动选择私有驱动。
4.分区注意：选择efi区(500m)，格式为fat32，标记/boot，挂载/boot/efi其余空间类似ubuntu自由分配,/home(100g+)大一点就行 
5. 进行安装前的基本设置，而后进入安装。注：这里设置密码最后不要带字母，因为发现后面输入终端sudo的时候，会显示输入错误，但是可以passwd改密码。
## <spam id="jump3">三.系统问题与修复<spam>
* 无法输出声音
	1. `aplay -l   //检查声卡驱动是否存在，结果中不能全是HDMI`
	
	   ![输出](https://i.loli.net/2020/12/13/E8SCJigZIU5HVDG.png )
	
	2. `sudo pacman -S sof-firmware  //上一步显示没有声卡时，安装声卡驱动`
	
	3. `sudo nano ~/.asoundrc  //编辑该文件`
	
	4. 在~/.asoundrc文件下写入下面内容:
	    `defaults.pcm.card 0`
	    `default.pcm.device 0`
	    `defaults.ctl.card 0`
	    care 0 /device 0由上面aplay -l输出显示声卡具体位置。重启即可。
* 无法显示表情和特殊符号，以致于终端主题和输入法显示不全。
此问题困扰我很长时间，最终在[xfce安装字体](https://blog.csdn.net/weixin_30895603/article/details/96442211)找到类似答案。
不同的是我使用的是***[Nerd-fonts](https://github.com/ryanoasis/nerd-fonts)***，在release选择想要的下载即可。
具体流程：
1. 解压下载好的字体包。
2. 拷贝字体文件夹到目录`usr/share/fonts`
3. 创建字体缓存：
	`mkfontscale  //在当前目录生成fonts.scale`
	`mkfontdir  //在当前目录生成fonts.dir`
	`fc-cache -fv  //重建字体缓存`
4. 设置-->外观-->字体-->选择目标字体即可，终端可以直接选择使用系统字体。
终于不会显示乱码了，值得一提的是：
设置-->Manjaro Settings Manager里面有语言包，可以安装额外的补充语音包。
注意：即使你更换字体，显示正常，在字体那里的预览依然不会显示正确，而这是xfce独有的，kde我未遇到。
## <span id="jump4">四.系统安装后的最初配置<span>
* 注意：本环节参考[Manjaro-KDE配置全攻略](https://zhuanlan.zhihu.com/p/114296129)，并对部分不适合xfce和本系统的环节进行了修正。
1. 换官方镜像源：
	`sudo pacman-mirrors -i -c China -m ranl //在弹窗中选择一个即可`
2. 换arch源：
	`sudo nano /etc/pacman.conf //编辑该文件，输入以下内容：`
	`[archlinuxcn]`
	`Server = http://mirrors.163.com/archlinux-cn/$arch`
	注意：arch的源更新比manjaro快，部分会不兼容。比如Qv2ray。
3. 更新系统：
	`sudo pacmam -Syyu`
4. 添加本地arch库和钥匙串(如果进行了步骤2)：
	`sudo pacman -S archlinuxcn-keyring`
5. 安装aur助手：
  * AUR具有很多第三方软件，属实NB。
	  `sudo pacman -S yaourt //安装yaourt，以后直接用yaourt安装软件。`
  * 注意:并不推荐`yay`命令，yay虽然简单，但很多软件都没办法搜到。
6. 安装Rime输入法：
* 安装fcitx5框架：`yaourt -S fcitx5-im //回车，默认安装fcitx5框架下所有内容`
* 配置fictx5环境变量：`nano ~/.pam_enviroment`
	输入：
	`INPUT_METHOD  DEFAULT=fcitx5`
	`GTK_IM_MODULE DEFAULT=fcitx5`
	`QT_IM_MODULE  DEFAULT=fcitx5`
	`XMODIFIERS    DEFAULT=\@im=fcitx5`
* 安装rime输入引擎：
  `yaourt -S fcitx5-rime`
  `这里需要重启一下`
    * 安装[clover输入方案](https://github.com/fkxxyz/rime-cloverpinyin/wiki/linux)：
      注意：这里直接yay安装会不成功，建议从[Github仓库](https://github.com/fkxxyz/rime-cloverpinyin/releases)下载源文件,解压，将文件夹的内容，移动到用户资料夹`~/.local/share/fcitx5/rime/`
    * 在用户资料夹下创建default.custom.yaml，内容：
      `patch:`
      `  "menu/page_size": 5`  `5表示候选字个数，可以为1-9`
      `  schema_list:`
      `    - schema: clover`
    * 配置主题[Fcitx5-Material-Color](https://github.com/hosxy/Fcitx5-Material-Color)
    * 注意：修改输出候选栏大小只能在主题文件相关配置中修改为9或8。
    * 重启，在fcitx5配置里面将rime移到第一。
 * oh-my-zsh，见前面blog，注意incr很卡，在输入https://时
    * 切换zsh主题为pure
      下载pure主题，解压，进入目录。
      `ln -s pure/pure.zsh-theme . #这一行好像不需要`
      `ln -s pure/async.zsh . `
      ～/.zshrc中将主题更改为`refined`
  * QQ/Tim
      弄了很久，最后才知道早就有大佬搞出来AppImage了
      [地址](https://github.com/askme765cs/Wine-QQ-TIM/releases/tag/v1.0)
      怕以后又没了，已经fork。
  * Qv2ray
      添加链接后始终无法上google，这个想了好久，在tg上也在官方找，没找到，最后猜想xfce不支持系统直接代理，于是chrome采用switchyomega（插件），shell使用on-my-zsh中zsh-proxy插件。
      毕竟以前就是即点即用的说🤕️。
  * wireguard
      `sudo uname -a  //查看系统内核，我的是5.9`
      `sudo yaourt -S linux-headers //安装对应版本`
      `sudo pacman -S wireguard-tools //安装wireguard`
      `将conf文件移到/etc/wireguard/目录下`
      `sudo wg-quick up name //启用name.conf`
      `sudo wg-quick down name //断开`
## <span id="jump5">五.美化<span>
* dock栏
      商店设置打开aur源，搜索plank，安装，启动，注意添加启动器的方法，在/usr/bin/下找，或者编辑应用程序中查看位置。Ctrl+鼠标右键进入首选项设置。
  
* 主题：
    直接使用Ubuntu下的gnome主题和图标。
    
* 全局菜单栏(vala-panel-appmenu)：
      ![上图](https://i.loli.net/2020/12/13/1xikJYDhtUI2wNr.png)
      商店搜索下面包：
      `vala-panel-appmenu-common`
      `vala-panel-appmenu-registrar`
      `vala-panel-appmenu-xfce`
      `appmenu-gtk-module`
      运行：
      `xfconf-query -c xsettings -p /Gtk/ShellShowsMenubar -n -t bool -s truexfconf-query -c xsettings -p /Gtk/ShellShowsAppmenu -n -t bool -s true`
  
    	  面板-->首选项-->关闭锁定面板，将其拖到上方并锁定.
    	  面板-->首选项-->项目-->配置面板插件
    	  举例：
    	  whisker菜单，双击，进入配置-->面板按钮-->图标，双击下方正方形，选择图标（推荐easyicon）
    	  分隔符-->样式：透明度，点击启用扩展成为空格键，调整面板高度和增添分隔符个数将日期移到中间。添加全局菜单。
    	  窗口管理器微调-->合成器。取消在弹窗下显示阴影和dock下显示阴影。
    	  LightDM桌面管理器设置锁屏和登录壁纸和显示图像，勾选使用用户头像，将壁纸文件移动到默认固定路径，不会出现黄色三角符号即代表成功。
    	  大致如此，其余微调不管了。
## <span id="jump6">六.尾声<span>
补充一下，重建Blog时依然出现了同样的错误，+v查看详细，最后赋予777解决。
结束了，不仅是这个折腾过程，也是这个学期。
想来大多时间在玩，在折腾这些，反而失去了根本。
接下来不会换系统了，静下心来学java。

补充：Linux中通用查找命令：

1. `find /* -name "*name*" //使用通配符增加范围`

2. flameshot gui 为flameshot的快捷建命令。

3. Swithyomega插件商店里的不能用，建议使用github中文件，然后使用命令行解压（oh-my-zsh自带extract万能解压命令），然后加载。