---
layout: post
title: openSUSE一键安装脚本（有更新）！
date: 2014-11-16 12:15:00 +0800
description: "openSUSE 一键安装脚本，在全新安装 openSUSE 之后，该脚本帮助你安装一些必要的软件包。比如多媒体播放相关的软件包, FireFox 的 Flash 插件, Google Chrome 浏览器,  Oracle JDK(不是 JRE，是适合于开发者用的 JDK), Oracle Java(就是 JRE，适合一般用户), wireshark, Virtual Box 虚拟机等等。"
tags: [openSUSE, openSUSE-One-Click-Installer]
---

##### 更新记录


###### 2015-7-25

- 启用 fdzh google chrome 镜像源。
- 启用 aliyun 相关 openSUSE 镜像源。
- 级 JDK 版本为 1.8.0_51。

###### 2015-4-18

- 分离安装 JDK 的逻辑为独立文件。
- 升级 JDK 版本为 1.8.0_45。

###### 2015-2-1

- 修复 javaFX 创建 MediaPlayer 会失败的故障。

###### 2014-12-6

- 新增安装酷我音乐 linux 原生客户端（非官方 ）。

###### 2014-11-22

- 转换 中文的用户目录为英文目录，原中文目录中的文件会自动迁移到英文目录中。转换的好处是可以很方便的在终端中进入用户目录，哪怕是在没有中文输入法的情况下。

###### 2014-11-20

- 安装 VLC多媒体播放器和硬件解码包，可以很方便的支持视频硬件解码（注意硬解我只测试了AMD显卡，而且需要已经安装AMD私有驱动）。
- 安装 支付宝安全控件（从支付宝官方网站下载并安装）。
- 安装 hotshots 屏幕截图软件，可以保存为多种格式，还可以添加文字注释，划线，箭头，上传图片到网络图床等。
- 安装 飞鸽传书的 linux 版本。


##### 功能介绍
在全新安装 openSUSE 之后，openSUSE 一键安装脚本帮助你安装一些必要的软件包。比如多媒体播放相关的软件包, FireFox 的 Flash 插件, Google Chrome 浏览器,  Oracle JDK(不是 JRE，是适合于开发者用的 JDK), Oracle Java(就是 JRE，适合一般用户), wireshark, Virtual Box 虚拟机等等。该脚本还支持无限重复运行，你无需担心重复运行会对系统产生危害。

该脚本已在 openSUSE 13.1 ， openSUSE 13.2 完成测试。无论是对于新手还是老手，该脚本都可以为你节约大量的时间。

该脚本具体的修改内容或者要安装的软件包如下：

- 禁用 DVD 光盘源。
- 启用 aliyun 的 openSUSE-Oss, openSUSE-Non-Oss, openSUSE-Update, openSUSE-Update 镜像源并禁用对应的官方源。
- 安装 gstreamer 相关插件，这样基于 phonon 框架的多媒体软件就可以播放受专利保护的多媒体文件了。
- 安装 Smplayer，同时还会自动安装 w32codec-all，这样 Smplayer 就可以播放 rmvb, wmv 等文件格式了。
- 安装 Flash Player，解决 Firefox 不能播放 flash 在线视频的问题。
- 安装 Google Chrome，同时解决访问不了 Google Chrome 源不能访问的问题。
- 安装 Quassel，一款先进的跨平台的分布式 IRC 聊天客户端，界面非常友好功能很强大。
- 安装 plasmoid-yawp，一款天气预报的等离子部件。
- 安装 FDesktopRecorder，一款基于 QT 编写的桌面录屏软件。
- 安装 kolourpaint，一款和微软绘图及其相似的 KDE 绘图工具。
- 安装 libreoffice 中文语言包，并解决 libreoffice 和 KDE 桌面主题不协调的问题。
- 安装 tomahawk，一款基于 QT 编写的非常美观的音乐播放软件。 
- 安装 酷我音乐 linux 原生客户端（非官方 ）。
- 安装 VLC多媒体播放器和硬件解码包，可以很方便的支持视频硬件解码（注意硬解我只测试了AMD显卡，而且需要已经安装AMD私有驱动）。
- 安装 支付宝安全控件（从支付宝官方网站下载并安装）。
- 安装 hotshots 屏幕截图软件，可以保存为多种格式，还可以添加文字注释，划线，箭头，上传图片到网络图床等。
- 安装 飞鸽传书的 linux 版本。
- 安装 Oracle JDK 最新版本。默认安装的是 JDK，不是 JRE，要安装 JRE 的请修改 ooci.conf 文件。
- 修复 javaFX 创建 MediaPlayer 会失败的故障。
- 安装 krusader，一款双面板的文件管理器，和 Total Commander 极其类似。
- 安装 osdlyrics，一款第三方歌词显示程序。它为 Linux 下的多款播放器提供类似 Windows 下 QQ 音乐的歌词显示功能，并能自动从网络上下载歌词
- 安装 rar， unrar，用于压缩，解压 rar 文件，同时 Ark 也支持 rar 文件了。
- 安装 p7zip，用于支持 7zip 压缩包，同时 Ark 也支持 7zip 文件了。
- 安装 unzip-rcc，安装了该包后 ark 打开一些 windows 下创建的 zip 文件（这些 zip 包中的文件名实际上是以 GBK 编码的）时不再乱码。
- 安装 libpng12-0，支付宝安全控件的依赖包。 
- 安装 wireshark，著名的网络抓包工具。 
- 安装 KDiff3，类似 Beyond Compare 的文件 / 文件夹比较工具。
- 安装 VirtualBox，Oracle 出品的虚拟机。
- 转换 中文的用户目录为英文目录，源中文目录中的文件会自动迁移到英文目录中。转换的好处是可以很方便的在终端中进入用户目录，哪怕是在没有中文输入法的情况下。
- 定义 zypper 相关的别名，比如 zin 对应于 sudo zypper in, zrm 对应于 sudo zypper rm -u。 
- 定义 xunlei-lixian 相关的别名，比如 lxlstoday 用于列出当天添加的 xunlei-lixian 下载任务。


##### 脚本源代码仓库

[https://github.com/redhatlinux10/openSUSE-One-Click-Installer](https://github.com/redhatlinux10/openSUSE-One-Click-Installer)

##### 使用方法

打开终端，运行下述代码就可以运行 openSUSE 一键安装脚本了:
{% highlight bash linenos %}
wget -nd -c -P ~ --no-check-certificate --no-cookies "https://raw.githubusercontent.com/redhatlinux10/openSUSE-One-Click-Installer/master/openSUSE-One-Click-Installer.sh" && sh ~/openSUSE-One-Click-Installer.sh
{% endhighlight %}

默认情况下 `openSUSE-One-Click-Installer.sh` 会执行上述提到的所有动作 / 安装所有软件包，如果你希望不要安装其中的某些软件包，可以通过定制 `ooci.conf` 文件来实现，因为本脚本执行的时候会从中读取配置项来决定是否要执行某个动作。首先你先运行下该脚本，在询问是否继续的时候选择`否`来取消本次执行，但是在这个时候该脚本其实已经把 `ooci.conf` 文件下载下来了，该配置文件和 `openSUSE-One-Click-Installer.sh` 同目录，都在你的用户家目录下。你可以修改 `ooci.conf` 文件的配置项来定制安装哪些软件包（不用怕，里面有详细的注释），然后再次运行 `openSUSE-One-Click-Installer.sh` 就可以了。

##### 汇报 bug 地址

[https://github.com/redhatlinux10/openSUSE-One-Click-Installer/issues](https://github.com/redhatlinux10/openSUSE-One-Click-Installer/issues)


